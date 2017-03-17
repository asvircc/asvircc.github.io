---
layout: post
title:  Test
date:   2017-03-12 10:10
categories: test
author: asvircc
---

~~~c

typedef struct
{
	pic_button_stru page1_btn[HOME_PAGE_1_BUTTON_CNT];//第一页按钮
	pic_button_stru page2_btn[HOME_PAGE_2_BUTTON_CNT];//第二页按钮

	struct Rect slide_area;
	NEXUS_SurfaceHandle slide_sur[HOME_PAGE_CNT];
	struct Rect select_page_pos;
	NEXUS_SurfaceHandle select_page[HOME_PAGE_CNT];
	int cur_page;    	 //当前页
	int offset_hor;      //水平偏移
	int offset_ver;      //垂直偏移

	slide_stru slide;
} s100_home_stru;


//首页页数
typedef enum
{
	HOME_PAGE_1,
	HOME_PAGE_2,
	HOME_PAGE_CNT
} HOME_PAGE_ENUM;


//图片按钮结构
typedef struct _pic_button_stru
{
	button_stru button;
	
	NEXUS_SurfaceHandle sur;         //非高亮图surface
	struct Rect src_area;    		 //非高亮图源位置
	NEXUS_SurfaceHandle hili_sur;    //高亮图surface
	struct Rect hili_src_area;       //高亮图源位置
	NEXUS_SurfaceHandle dst_sur;     //绘制时使用的目标surface
	struct Rect dst_area;			 //绘制时使用的目标区域
} pic_button_stru;

~~~



----
