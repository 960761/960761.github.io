
## 参考链接:  
http://www.dbaref.com/autosys-jobs  

https://www.autosysguide.com/   

http://autosys-tutorials.blogspot.com/2011/04/autosys-quick-reference.html  

http://autosys-tutorial-beginner.blogspot.com/2015/09/chapter-4-working-with-jobs.html  

Autosys是一个成熟的产品或软件，其功能简单讲，是一个定时工作器。  
可以根据某些事件，或者条件，或者定时，执行某些动作。

## 核心词：  
job:  要执行的操作  
JIL:  特有的一种语言，用来定义job，称为jil脚本文件

## demo
1. jil脚本文件 “helloJob.jil”  
~~~
insert_job:helloJob
machine :unix machine name
owner :username
command :echo “Hello this a welcome command job”
~~~
2. 采用jil命令运行jil脚本文件：  
jil < test.jil

job由许多不同属性构成, 最重要的三点:when, where, if

when  
指定开始时间，星期，　日历，watched file   
where  
机器名，　指运行该job的机器   
if  
运行条件 
 
例：
(1). 创建 Command Jobs
~~~
/* ----------------- XXXX_TES_RECON ----------------- */ 
insert_job: XXXX_TES_RECON  job_type: c 
box_name: SG_TES_DATA_BX
command: $SG_BIN_DIR/recon_dt.sh PAPRO SG
machine: SG_TES_VM
permission: gx,mx,me
date_conditions: 1
days_of_week: tu, we, th, fr, sa
start_times: "05:00"
condition: s(SG_TES_DATA_ROFILE)
description: "genertate recon for ROFILE"
std_out_file: $SG_TES_LOG_DIR/Autosys/$AUTO_JOB_NAME.out
std_err_file: $SG_TES_LOG_DIR/Autosys/$AUTO_JOB_NAME.err
alarm_if_fail: 1
profile: /app/TES/SG/config/autosys.profile 

这个脚本将创建名为：XXXX_TES_RECON的job
运行时间： 每周二 -- 周六 05：00启动，依赖条件 SG_TES_DATA_ROFILE SUCCESS
运行的机器：SG_TES_VM
job 类型：command
shell ：$SG_BIN_DIR/recon_dt.sh PAPRO SG
box：SG_TES_DATA_BX
~~~

Job 状态
~~~
Status	        Description
INACTIVE	JOB 未运行
STARTING	JOB初始化中
RUNNING	    JOB运行中
SUCCESS	    JOB运行成功
FAILURE	    JOB运行失败
TERMINATED	 JOB 在running时被kill 
RESTART	     其它硬件或者应用问题导致的job需要被重启
QUE_WAIT	 job达到启动条件，但由于其它原因导致暂时无法启动
ACTIVATED	 适用于box job, 指box已经在RUNNING，但job还未能启动
ON_HOLD	 job被设置ON_HOLD, 只有收到JOB_OFF_HOLD命令时才能运行
ON_ICE	 job被设置ON_ICE, 只有收到JOB_OFF_ICE命令时才能恢复运行
~~~
