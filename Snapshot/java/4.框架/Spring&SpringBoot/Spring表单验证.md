1. 在类路径下添加对Java 校验API的实现的jar包，即引入相关依赖。（这个貌似不用加，在某些情况下需要加进去）

   ```xml
   <dependency>
       <groupId>org.hibernate</groupId>
       <artifactId>hibernate-validator</artifactId>
       <version>6.0.13.Final</version>
   </dependency>
   ```

2. 为校验对象添加校验注解。

```java
public class DocumentInfoDto {
	
	@NotNull
	private String docName;

	@NotNull
	private String docType;

	private String docDesc;

	@NotNull
	private String discriminate;
	
	@NotNull
	private String fileType;
	
	private String docNum;
	
	@NotNull
	private String docFrom;
	
	private List<String> filePaths;

}
```

3. 应用校验功能

```java
@RequestMapping(value = "/submitDocumentInfo", method = RequestMethod.POST)
	public String submitDocumentInfo(
			@Valid DocumentInfoDto documentInfo,//应用校验功能
			Errors errors,
			@RequestParam(value = "file", required = false) MultipartFile[] files) {
        if (errors.hasErrors()) {//检查是否能够通过校验
			return "document/documentCreate";
		}
		return "document/documentList";
	}
```

3. 使用Spring提供的标签库。一共提供了两个标签库，**一个用来渲染Form标签**，一个用来提供一些工具类标签，第一个标签库更加常用。

```jsp
<?xml version="1.0" encoding="UTF-8" ?>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="access" uri="/WEB-INF/tld/access.tld"%>
<%@ taglib prefix="page" uri="/WEB-INF/tld/pager.tld"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
<!DOCTYPE html>
<html>
<head>
</head>
<body>
	<nav class="breadcrumb">
		<i class="Hui-iconfont">&#xe67f;</i> 首页 <span class="c-gray en">&gt;</span>
		文件管理 <span class="c-gray en">&gt;</span> 文件列表管理 <span
			class="c-gray en">&gt;</span> 新建文件<a class="btn btn-success radius r"
			style="line-height: 1.6em; margin-top: 3px;"
			href="javascript:location.replace(location.href);" title="刷新"><i
			class="icon-refresh icon-large">刷新</i></a>
	</nav>
	<div class="page-container">
		<%@include file="../common/alert.jsp"%>
		<div class="dataTables_wrapper no-footer">
			<form:form 
                method="POST" 
                commandName="documentInfo"
				action="${pageContext.request.contextPath}/document/submitDocumentInfo"
				enctype="multipart/form-data">
				<table
					class="table  table-border table-bordered table-hover table-bg table-sort">

					<tr>
						<td class="text-r success"><label>选择附件</label></td>
						<td>
                            <input type="file" 
                                   name="file" 
                                   onchange="PreviewImage(this)" /> 
						</td>
					</tr>

					<tr>
						<td class="text-r success" width="10%"><label>文件名称</label></td>
						<td>
                            <form:input path="docName" class="input-text" />
						</td>
					</tr>

					<tr>
						<td class="text-r success"><label>文件类型</label></td>
						<td><form:select path="docType" class="select"
								style="width:150px; height: 31px;" id="docType"
								>
								<form:options items="${docTypeList}" itemValue="value" itemLabel="name"/>
							</form:select> 
							</td>
					</tr>

					<tr>
						<td class="text-r success"><label>来文/收文机关</label></td>
						<td><form:input path="docFrom" class="input-text" /> <%-- <input class="input-text" name="docFrom" value="${ documentInfo.docFrom }"/> --%>
						</td>
					</tr>
					<tr>

						<td class="text-r success"><label>收/发文</label></td>
						<td><form:select path="discriminate" class="select"
								style="width:150px; height: 31px;" id="discriminate"
								>
								<form:options items="${discriminateList}" itemValue="value" itemLabel="name"/>
							</form:select> 
							</td>
					</tr>
					<tr>
						<td class="text-r success"><label>数字化文件类型</label></td>
						<td><form:select path="fileType" class="select"
								style="width:150px; height: 31px;" id="fileType"
								>
								<form:options items="${fileTypeList}" itemValue="value" itemLabel="name"/>
							</form:select> 
							</td>
					</tr>
					<tr>
						<td class="text-r success"><label>文件描述</label></td>
						<td><form:textarea path="docDesc" class="textarea" /> 
						</td>
					</tr>
				</table>

				<table class="table table-hover table-bg table-sort">
					<tr>
						<td class="text-c">
							<div style="width: 600px; margin: 0 auto">
								<input class="btn btn-primary radius" value="提交信息"
									type="submit" />
							</div>
						</td>
					</tr>
				</table>

			</form:form>
			<div class="loadingDiv">
				<div class="loadingBg"></div>
				<img class="loadingImg" src="/resources/images/loading.gif">
			</div>
		</div>
	</div>
</body>
</html>
```



附1：

JAVA 校验 API 提供的相关注解

![Snip20190408_1](/Users/liuzhe/Documents/project/LZ/TechSnapshot/Snapshot/web/4.Framework/assets/Snip20190408_1.png)