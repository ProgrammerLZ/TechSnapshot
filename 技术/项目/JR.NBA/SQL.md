# 会员招募

### 累积用户、累积认证用户、累积教师认证用户

```sql
SELECT
	COUNT( user_wx.USER_ID ) userCnt,
	COUNT( user_mobile.USER_ID ) mobileAuthCnt,
	COUNT( user_teacher_info.USER_ID ) teacherAuthCnt
FROM
	user_wx user_wx
	LEFT JOIN user_mobile user_mobile ON user_wx.USER_ID = user_mobile.USER_ID
	LEFT JOIN user_teacher_info user_teacher_info ON user_wx.USER_ID = user_teacher_info.USER_ID
```



### 七日平均新用户、七日平均认证、七日平均教师认证

```sql
SELECT
	ROUND( ( COUNT( user_wx.USER_ID ) / 7 ) ) userCnt,
	ROUND( ( COUNT( user_mobile.USER_ID ) / 7 ) ) mobileAuthCnt,
	ROUND( ( COUNT( user_teacher_info.USER_ID ) / 7 ) ) teacherAuthCnt 
FROM
	user_wx user_wx
	LEFT JOIN user_mobile user_mobile ON user_wx.USER_ID = user_mobile.USER_ID
	LEFT JOIN user_teacher_info user_teacher_info ON user_wx.USER_ID = user_teacher_info.USER_ID 
WHERE
	user_wx.INS_DATE BETWEEN '2019-07-21' 
	AND '2019-07-27' #查询七日的平均
```



### 七日日活

```sql
SELECT
	SUM( activity ) userActivity
FROM
	(
SELECT
	COUNT( DISTINCT USER_ID ) activity
FROM
	user_log_20190722 
WHERE
	URI = '001' #枚举
UNION ALL
SELECT
	COUNT( DISTINCT USER_ID ) activity
FROM
	user_log_20190728 
WHERE
	URI = '001' #枚举
	
	) AS allData
```

### 七日平均日活

```sql

```



### 本月新增用户、本月认证用户

（是否包括教师认证）

```sql
SELECT
	COUNT( user_wx.USER_ID ),
	COUNT( user_mobile.USER_ID )
FROM
	user_wx user_wx
	LEFT JOIN user_mobile user_mobile ON user_wx.USER_ID = user_mobile.USER_ID 
WHERE
	DATE_FORMAT( user_wx.INS_DATE, '%Y%m' ) = DATE_FORMAT( CURRENT_DATE, '%Y%m' )
```



# 会员等级分布

## 各等级用户数量

```mysql
SELECT
	ML.LEVEL_TITLE,
	ML.LEVEL_NAME,
	COUNT( ML.MEMBER_LEVEL_ID )
FROM
	user_wx UX
	LEFT JOIN user_integral UI ON UX.USER_ID = UI.USER_ID
	LEFT JOIN member_level ML ON UI.INTEGRAL_TOTAL BETWEEN ML.INTEGRAL_MIN 
	AND ML.INTEGRAL_MAX 
GROUP BY
	ML.MEMBER_LEVEL_ID
```



## 新增用户中认证用户的占有率

```sql

```



## 各等级用户占比

```sql

```

## 各等级用户新增用户的数量

```sql

```

# 积分情况

## 共计发放积分数量

```sql
select sum(INTEGRAL_TOTAL) from user_integral;

```

## 共计消耗积分数量

```sql

```

## 共计发放积分总人数

```sql
select count(USER_ID) from user_integral;

```

## 积分消耗总人数

```

```

## 活动积分占比

```sql

```

## 签到积分占比

```sql

```

## 内容阅读占比

```sql

```

## 积分发放量和消耗量

```

```

## 积分发放人数和消耗积分人数

```

```

# 内容阅读情况

## 视频

### 当月视频播放top10

```mysql
SELECT
	VIDEO_TITLE,
	BROADCAST_CNT
FROM
	video_info 
WHERE DEL_FLAG != 1
ORDER BY
	BROADCAST_CNT DESC 
	LIMIT 10

```



### 月标签热度

```mysql

```



### 视频月播放量

```mysql
SELECT
COUNT( vbl.VIDEO_ID ) playCount,
vi.VIDEO_TITLE  videoTitle
FROM
	video_brordcast_log vbl
	LEFT JOIN video_info vi ON vbl.VIDEO_ID = vi.VIDEO_ID 
WHERE
	( vbl.INS_DATE BETWEEN '2019-07-01' AND '2019-08-01' ) 
	AND vi.VIDEO_ID IS NOT NULL 
GROUP BY
	vbl.VIDEO_ID

```



## 教案

教案需要添加一个下载的字段

## 课程

### 指定日期课程参与的总人数

```sql
SELECT
	COUNT(*)
FROM
	user_course_info 
WHERE
	DATE_FORMAT(APPLY_TIME,'%Y-%m-%d')  = '2019-07-26'

```

## 用户黑名单

### 黑名单总人数

```mysql
select count(USER_ID) from user_wx where BLACK_FLAG=1;


```

### 每月拉黑人数

```mysql
select count(USER_ID) from user_wx where BLACK_FLAG=1;!查不出来

```

# TTT活动数据

## 线上活动数据统计

### 线上活动参加总人数

```mysql
select count(USER_ID) from activity_user as u 
inner join activity_main as m on u.ACTIVITY_ID=m.ACTIVITY_ID   
where m.ACTIVITY_TYPE=1;

```

### 线上活动签到签满人数

```mysql

```

### 课程完成人数

```mysql

```



### 线上活动领取礼包人数

```mysql
select count(USER_ID) from activity_user_gift as g 
inner join activity_main as m on g.ACTIVITY_ID=m.ACTIVITY_ID 
where GIFT_RECEIVE_FLAG=1 and m.ACTIVITY_TYPE=1;

```

### 线上活动通过考试人数

```mysql
select count(USER_ID) from activity_user as u 
inner join activity_main as m on u.ACTIVITY_ID=m.ACTIVITY_ID   
where u.EXAM_SCORE>=60 and m.ACTIVITY_TYPE=1 ;

```

## 线下活动数据统计

### 线下活动参与人数

```mysql
select count(USER_ID) from activity_user as u 
inner join activity_main as m on u.ACTIVITY_ID=m.ACTIVITY_ID   
where m.ACTIVITY_TYPE=2;

```

### 线下活动签到签满人数

```mysql
select s.ACTIVITY_ID, count(USER_ID) from activity_user_sign as s 
inner join activity_main as m on s.ACTIVITY_ID=m.ACTIVITY_ID 
where m.ACTIVITY_TYPE=2 group by s.USER_ID having count(USER_ID)>=6;

```

### 线下活动考试及格人数

```mysql
select count(USER_ID) from activity_user as u 
inner join activity_main as m on u.ACTIVITY_ID=m.ACTIVITY_ID   
where u.EXAM_SCORE>=60 and m.ACTIVITY_TYPE=2 ;

```

### 线下活动领取礼包人数

```mysql
select count(USER_ID) from activity_user_gift as g 
inner join activity_main as m on g.ACTIVITY_ID=m.ACTIVITY_ID 
where GIFT_RECEIVE_FLAG=1 and m.ACTIVITY_TYPE=2;

```

# 客诉与咨询

## 用户反馈，投出意见数量

```mysql

```

## 投诉问题占比多少

```mysql

```

