# 课程自动回复梳理

### 基本流程

+ 登陆获取用户信息
+ 获取所有的话术用于前端搜查（taskId,next_taskId）
+ 获取聊天记录(初始化问候语)
+ 点击查看获取下一条话术
	+ 根据上一条的任务id 查询 下一条的任务id
    + 根据任务id查询相应的话术
    + 根据查到的结果，查看下一条任务是否自动显示
	+ 把聊天记录保存到数据库

### 实现梳理
> 一. 登陆

通过登陆获取userId，userName，token。用以获取聊天记录，课程查询，接口调用

> 二. 获取课程信息 以及 所有的话术

课程信息 
```
{
	courseId: "e1995108-6c13-4293-8468-ce513e4771c8", // 课程id
	createdAt: "2019-11-17 21:33:01", // 创建时间
	firstTaskId: "7a788660-9314-4be4-8bfe-1ad31cd04032", // 第一次任务id
	id: "28652c3c-e0f6-4d0b-8391-f56277f27d30", // id
	name: "通往人工智能的捷径：Python", // 课程名称
}
```
所有的话术
```
{
	classId: "28652c3c-e0f6-4d0b-8391-f56277f27d30"  // 课程id
	createdAt: "2019-11-17T13:33:01.000+0000" // 创建时间
	id: "028a1ed2-60f6-4243-a40a-940abb6551e4" // 任务id
	level1: ""
	level2: ""
	next: "adb0704a-b17b-447f-adc9-aa3f0e2a8bac" // 下一个任务的id
	num: 5 // 排序
	props: "{"text":"昨天我们聊了人工智能的事情，今天我们讲讲Python。"}" // 话术
	role: "Teacher" // 角色决定聊天显示左边右边
	type: "text" // 类型 text，img, button
	updateAt: "2019-11-17T13:33:01.000+0000"
}
```
> 三. 根据登陆人获取聊天记录

传递参数
```
{
	token: jF69WkOwKB6z3erLyJs，
	class_id: 28652c3c-e0f6-4d0b-8391-f56277f27d30
}
```

参数返回的是聊天记录（遍历根据taskId找到话术）
```
{
	appData: null
	classId: "28652c3c-e0f6-4d0b-8391-f56277f27d30" 
	codeLineCount: 0 
	courseId: "e1995108-6c13-4293-8468-ce513e4771c8"
	createdAt: "2020-03-27T01:21:40.000+0000"
	endTime: "2020-03-27T01:22:01.000+0000"
	historyId: "a28dedba-e076-4ed3-82f6-aa3a7502d371"
	id: "99000b21-dbb2-4206-976c-44f13509a4d0"
	progress: 4 
	result: ""
	startTime: "2020-03-27T01:21:40.000+0000"
	status: 30
	taskId: "028a1ed2-60f6-4243-a40a-940abb6551e4" // 任务id指向话术中的任务
	updateAt: "2020-03-27T01:22:01.000+0000"
	userId: "unionid_oBB9ps-6FEGuYvMXDv4QdEGNV0yo"
	variables: "{}"
	version: 1
	wordCount: 27
}
```

> 四. 点击获取下一聊天记录（用来保存聊天天记录以及下一个话术）

传递参数
```
{
	class_id: 28652c3c-e0f6-4d0b-8391-f56277f27d30 // 课程id
	task_id: 7de8b0da-c1e4-4bc9-bd0b-02d5197c6df5 // 当前任务id
	next_class_id: 28652c3c-e0f6-4d0b-8391-f56277f27d30 // 下一个课程id
	next_task_id: 24cb6028-c37d-4918-b4a2-3642e5a0c78a //  下一个课任务id
	word_count: 55 // 任务的排序
}
```
返回参数（获取话术，记录聊天记录）
```
{
	taskId: "51d0cf26-a867-47e2-bf32-948954ba3983" // 任务id
	wordCount: 85 // 任务的排序
}
```






