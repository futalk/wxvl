#  某开源cms 0day挖掘   
用户9528  安全洞察知识图谱   2025-05-13 00:30  
  
**免责声明**  
 由于传播、利用本公众号所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号安全洞察知识图谱及作者不为**此**  
承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
## 1项目介绍  
```
作者：用户9528
原文链接：https://xz.aliyun.com/news/17601
```  
  
简介  
  
CicadasCMS是用springboot+mybatis+beetl开发的一款CMS，支持自定义内容模型、模板标签、全站静态化等功能。  
  
  
![Pasted image 20250331211235.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkKUTRhTKGq304kjSdGPia4BUq6hMBibDTUjET86sh9vXxITTzib1dZBCUQ/640?wx_fmt=jpeg "")  
  
  
  
![Pasted image 20250331211156.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkxgEcnwkBFCibFOvQwRlLjRZJNyba0cAqkWz3kKf8mTf6V4LD0Iuy73Q/640?wx_fmt=jpeg "")  
### 漏洞挖掘  
  
sql注入（成功）  
  
  
漏洞发生位置在com.zhiliao.module.web.cms.ContentController#save：  
  
```
    @RequiresPermissions("content:save")
    @RequestMapping("/save")
    @ResponseBody
    public String save(TCmsContent content, HttpServletRequest request,
                       @RequestParam(value = "tag",required = false) String[] tags
                      ) throws SQLException {
        TCmsCategory category =categoryService.findById(content.getCategoryId());
        TCmsModel cmsModel = modelService.findById(category.getModelId());
        List<TCmsModelFiled> cmsModelFileds = modelFiledService.findModelFiledListByModelId(category.getModelId());
        UserVo userVo = UserUtil.getSysUserVo();
        content.setSiteId(userVo.getSiteId());
        content.setUserId(userVo.getUserId());
        content.setInputdate(new Date());
        content.setModelId(category.getModelId());
        /*Jin：使用Map接收：遍历获取自定义模型字段*/
        Map<String, Object> formParam =Maps.newHashMap();
        for (TCmsModelFiled filed : cmsModelFileds) {
            if(filed.getFiledClass().equals("checkbox")||filed.getFiledClass().equals("image")) {
                String[] filedValue = request.getParameterValues(filed.getFiledName());
                if (filedValue!=null) {
                    formParam.put(filed.getFiledName(), filedValue);
                }
            }else {
                String filedValue = request.getParameter(filed.getFiledName());
                if (!StrUtil.isBlank(filedValue)) {
                    formParam.put(filed.getFiledName(), filedValue);
                }
            }
        }
        if(content.getContentId()!=null)
            return contentService.update(content,cmsModel.getTableName(),cmsModelFileds,formParam,tags);
        return contentService.save(content,cmsModel.getTableName(),formParam,tags);
    }
```  
  
  
save方法接受了一个content对象为参数，这个content对象包含主键contentId等信息，formParam对象为一个新建的hashMap，用于保存表单数据的键值对，表示了一些扩展字段和其对应值，那么在这个逻辑中，如果contentId不为空，则调用com.zhiliao.module.web.cms.service.ContentService#update更新数据，否则调用com.zhiliao.module.web.cms.service.ContentService#save进行数据保存：  
  
  
![Pasted image 20250331052259.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkMu7UuyqUeercRhJ3fykamHXPEpeiaVXE8PXjk1RpHxDFQaI3akfqQicA/640?wx_fmt=jpeg "")  
  
  
  
com.zhiliao.module.web.cms.service.impl.ContentServiceImpl#update(com.zhiliao.mybatis.model.TCmsContent, java.lang.String, java.util.List<com.zhiliao.mybatis.model.TCmsModelFiled>, java.util.Map<java.lang.String,java.lang.Object>, java.lang.String[])：  
```
    @CacheEvict(cacheNames ="cms-content-cache",allEntries = true)
    @Transactional(transactionManager = "masterTransactionManager",rollbackFor = Exception.class)
    @Override
    public String update(TCmsContent content, String tableName, List<TCmsModelFiled> cmsModelFileds, Map<String, Object> formParam, String[] tags) throws SQLException {
        /*初始化文章的推荐和顶置为false*/
        content.setRecommend(CmsUtil.isNullOrEmpty(content.getRecommend())?false:true);
        content.setTop(CmsUtil.isNullOrEmpty(content.getTop())?false:true);
        content.setUpdatedate(new Date());
        if(contentMapper.updateByPrimaryKeySelective(content)>0) {
             /*创建lucene索引*/
            if(categoryService.findById(content.getCategoryId()).getAllowSearch()) {
                IndexObject indexObject = new IndexObject();
                indexObject.setId(content.getContentId().toString());
                indexObject.setTitle(content.getTitle());
                indexObject.setKeywords(content.getKeywords());
                indexObject.setDescription(content.getDescription());
                indexObject.setPostDate(DateUtil.formatDateTime(content.getInputdate()));
                indexObject.setUrl(this.httpProtocol + "://" + ControllerUtil.getDomain() + "/front/" + content.getSiteId() + "/" + content.getCategoryId() + "/" + content.getContentId());
                luceneService.update(indexObject);
            }
            /*保存和文章管理的Tag*/
            if (tags != null)
                for (String tag : tags) {
                    tagService.save(content.getContentId(), tag);
                }
            this.SaveModelFiledParam(formParam,content,tableName,cmsModelFileds);
            return JsonUtil.toSUCCESS("操作成功", "", true);
        }
        return JsonUtil.toERROR("操作失败");
    }
```  
  
又调用了com.zhiliao.module.web.cms.service.impl.ContentServiceImpl#SaveModelFiledParam进行数据保存：  
  
  
![Pasted image 20250331052841.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkicic97y1nX4UdZWbBVxGicibRIBF5Veysa4LUhTzibicgkBWMZgmycQjxviag/640?wx_fmt=jpeg "")  
  
  
  
接着跟进com.zhiliao.module.web.cms.service.impl.ContentServiceImpl#SaveModelFiledParam方法：  
```
 @Transactional(transactionManager = "masterTransactionManager",rollbackFor = Exception.class)
    public int SaveModelFiledParam(Map<String, Object> formParam,TCmsContent content,String tableName,List<TCmsModelFiled> cmsModelFileds) throws SQLException {
        if(!CmsUtil.isNullOrEmpty(formParam)) {
            String exeSql;
            Connection  connection = dataSource.getConnection();
            Statement statement = connection.createStatement();
            String selectSql = "select * from t_cms_content_"+tableName+" where content_id="+content.getContentId();
            ResultSet resultSet =statement.executeQuery(selectSql);
            /*判断内容扩展表是否存在数据*/
            if(!resultSet.next()) {
                exeSql = "insert into t_cms_content_" + tableName.trim() + " set ";
                exeSql += "`content_id`=" + content.getContentId() + ", ";
                for (Map.Entry<String, Object> entry : formParam.entrySet()) {
                    exeSql += "`" + entry.getKey() + "`";
                    if(CmsUtil.isNullOrEmpty(entry.getValue()))
                        return 0;
                    if (entry.getValue() instanceof Integer) {
                        exeSql += "=" + entry.getValue() + ", ";
                    } else if (entry.getValue().getClass().isArray()) {
                        /*遍历字符数组，数组来源为checkbox和多图上传*/
                        String[] result = (String[]) entry.getValue();
                        if (result != null) {
                            String tmp = "";
                            for (String value : result) {
                                if(StrUtil.isBlank(value))
                                    continue;
                                tmp += value + "#";
                            }
                            exeSql += "='" + tmp.substring(0, tmp.length() - 1) + "', ";
                        }
                    } else {
                        exeSql += "='" + entry.getValue() + "', ";
                    }
                }
                /*执勤Sql前截取最后面的空格和英文逗号，并加上‘;’*/
                exeSql = exeSql.substring(0, exeSql.length() - 2) + ";";
                int status= statement.executeUpdate(exeSql);
                statement.close();
                connection.close();
                return status;
            }else {
                exeSql = "UPDATE t_cms_content_" + tableName.trim() + " set ";
                      /*遍历Map保存扩展数据表,代码有些复杂*/
                for (TCmsModelFiled filed : cmsModelFileds) {
                    if(CmsUtil.isNullOrEmpty(formParam.get(filed.getFiledName()))) continue;
                    exeSql += "`" + filed.getFiledName() + "`";
                    if (formParam.get(filed.getFiledName()) instanceof Integer) {
                        exeSql += "=" + formParam.get(filed.getFiledName()) + ", ";
                    } else if (!CmsUtil.isNullOrEmpty(formParam.get(filed.getFiledName())) && formParam.get(filed.getFiledName()).getClass().isArray()) {
                    /*遍历字符数组，数组来源为checkbox和多图上传*/
                        String[] result = (String[]) formParam.get(filed.getFiledName());
                        if (result != null) {
                            String tmp = "";
                            for (String value : result) {
                                tmp += value + "#";
                            }
                            exeSql += "='" + tmp.substring(0, tmp.length() - 1) + "', ";
                        }
                    } else {
                        exeSql += "='" + formParam.get(filed.getFiledName()) + "', ";
                    }
                }
                /*截取最后面的空格和英文逗号，并加上‘;’*/
                exeSql = exeSql.substring(0, exeSql.length() - 2) + "where `content_id`=" + content.getContentId() + ";";
                int status =statement.executeUpdate(exeSql);
                statement.close();
                connection.close();
                return status;
            }
        }
        return 0;
    }
```  
  
那么这里执行的逻辑是：首先进行非空判断，接着遍历表单数据并且动态拼接到sql执行语句中，最后进行执行，显然这里是存在sql注入漏洞：  
  
  
![Pasted image 20250331053306.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkDPPtMSl5we6icLZhRibLxvrnMr9iaBrS254FO13J0licFO4sVqics8HmI6Q/640?wx_fmt=jpeg "")  
  
  
  
文件上传（失败）：  
  
com.zhiliao.common.upload.UploadComponent#uploadFile：  
```
    public UploadBean uploadFile(MultipartFile multipartFile, HttpServletRequest request){
        TSysAttachment attachment = new TSysAttachment();
        /** 获取用户会话 **/
        UserVo userVo  = (UserVo)request.getSession().getAttribute(CmsConst.SITE_USER_SESSION_KEY);
        if(userVo!=null) {
            attachment.setUserId(userVo.getUserId().toString());
            attachment.setUsername(userVo.getUsername());
        }
        attachment.setUploadDate(new Date());
        attachment.setUploadIp(ControllerUtil.getRemoteAddress(request));
        attachment.setFileSize(Float.valueOf(multipartFile.getSize())/1024);
        attachment.setFileExtname(multipartFile.getContentType());
        attachment.setFileKey(UUID.randomUUID().toString().replace("-",""));
        attachment.setOriginalFilename(multipartFile.getOriginalFilename());
        /*创建uploadBean*/
        UploadBean result = new UploadBean();
        String fileType = this.getFileType(attachment.getOriginalFilename());
        String fileName = this.getFileName(fileType) ;
        String newName =this.getNewFileName(fileName);

        if (!multipartFile.isEmpty()) {
            if (!Boolean.parseBoolean(qiniuUpload)) {
                File file = new File(this.getUploadPath() + newName);
                /*如果不存在就创建*/
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }
                try {
                    this.writeFile(multipartFile.getBytes(), file);
                    attachment.setFilePath(newName);
                    attachment.setFileName(fileName);
                    if(Boolean.parseBoolean(enableFullPath)) {
                        result.setFileUrl(request.getScheme() + "://" + ControllerUtil.getDomain() + "/res/" + attachment.getFileKey() + "." + fileType);
                    }else {
                        result.setFileUrl("/res/" + attachment.getFileKey() + "." + fileType);

                    }
                    attachmentService.save(attachment);
                } catch (Exception e) {
                    throw new ApiException(e.getMessage());
                }
            }else {
                String qiniuFileResult = QiniuUtil.upload(accessKey, secretKey, bucketname, multipartFile);
                if (!StrUtil.isBlank(qiniuFileResult)) {
                    String fileKey = JSON.parseObject(qiniuFileResult).getString("key");
                    String fileUrl = domain + "/" + fileKey;
                    if (StrUtil.getExtensionName(fileName).equals("jpg") || StrUtil.getExtensionName(fileName).equals("JPG") || StrUtil.getExtensionName(fileName).equals("png") || StrUtil.getExtensionName(fileName).equals("PNG") || StrUtil.getExtensionName(fileName).equals("jpeg") || StrUtil.getExtensionName(fileName).equals("JPEG")) {
                        fileUrl += "?imageslim";
                    }
                    result.setFileUrl(fileUrl);
                }
            }
            return result;
        }else{
            throw new ApiException("上传文件不能为空！");
        }
    }
```  
  
这里的newName是从this.getNewFileName(fileName);得到的，fileName又是通过this.getFileName(fileType) ;获得的：  
  
  
![Pasted image 20250331054728.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibk3pW7iaH7F804icoCEHdMBX5ia6USdKeTVh7QHAPnjhZX2PryIVPiaM5zIQ/640?wx_fmt=jpeg "")  
  
  
  
最开始fileType又是通过this.getFileType(attachment.getOriginalFilename());获得的：  
  
  
![Pasted image 20250331055057.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkXia6DRP9ANyjic3gqXHcQKSFTqyapdV8JXx2F3YiaNYNpyVsP1farKibdw/640?wx_fmt=jpeg "")  
  
  
  
那么跟进com.zhiliao.common.upload.UploadComponent#getFileType：  
```
    public String getFileType(String fileName) {
        String type = fileName.substring(fileName.lastIndexOf(".") + 1);
        return type;
    }
```  
  
那么这里是使用了lastIndexof函数，这样的话看上去后文件的类型是不可控的。  
### 总结  
### 这套源码是很老了，整体难度不大，非常适合新手进行学习。  
## 2免费社区  
  
安全洞察知识图谱星球是一个聚焦于信息安全对抗技术和企业安全建设的话题社区，也是一个  
**[免费]**  
的星球，欢迎大伙加入积极分享红蓝对抗、渗透测试、安全建设等热点主题  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rh8aia4mibs0I8I42MrYYOSE2DVEpVpPHvxufMGR0yufpgouwIXEl7H5eLm0MgolGFQMDFIrKLTxaYIQ/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PDVoxXx6Rhic2eSUB7oxwhHPcGA4rxBibkzK2yZDWbEbIFUicsiaIvKBicJpoAGWD0TsCuglicbPcQsyKrmIibvHiaGWDA/640?wx_fmt=jpeg&from=appmsg "")  
  
