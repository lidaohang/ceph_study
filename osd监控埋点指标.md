### 1. WBThrottle

| 监控类型   |      监控项      |  说明	 |
|----------|:-------------:|:-------------:|
| perf dump WBThrottle |  bytes_dirtied         | 脏数据大小     		  |
|                      |  bytes_wb              | 写入数据大小   		  |
|                      |  ios_dirtied           | 脏数据操作	          |
|                      |  ios_wb                | 写操作               |
|                      |  inodes_dirtied        | 等待写入的条目        |
|                      |  inodes_wb             | 写记录               |

### 2. filestore
| 监控类型   |      监控项      |  说明  |
|----------|:-------------:|:-------------:|
| perf dump filestore  |  journal_queue_max_ops         | 日志队列中的最大操作	    	|
| 		       		   |  journal_queue_ops         	| 日志队列中的操作大小	    	|
| 		       		   |  journal_ops         			| 日志请求数	    			|
| 		               |  journal_bytes         	    | 日志大小	    			|
| 		       		   |  journal_latency.avgcount      | 日志等待队列平均大小	    	|
| 		       		   |  journal_latency.sum      		| 日志等待队列总大小	    	|
| 		       		   |  journal_latency.avgtime       | 日志等待队列平均时间	    	|
| 		       		   |  journal_wr      				| 日志读写io	    			|
| 		       		   |  journal_wr_bytes.avgcount     | 日志读写大小的队列平均数量	|
| 		       		   |  journal_wr_bytes.sum      	| 日志读写大小的队列总数	    |
| 		       		   |  journal_full      			| 日志写满	   				|
| 		       		   |  committing     				| 正在提交数量	       		|
| 		       		   |  commitcycle_interval.avgcount | 提交之间的间隔队列平均数量	|
| 		       		   |  commitcycle_interval.sum     	| 提交之间的间隔队列总数	    |
| 		       		   |  commitcycle_interval.avgtime  | 提交之间的间隔队列平均时间	|
| 		       		   |  commitcycle_latency.avgcount  | 提交延迟队列平均数量	    	|
| 		       		   |  commitcycle_latency.sum     	| 提交延迟队列总数	      		|
| 		       		   |  commitcycle_latency.avgtime   | 提交延迟队列平均时间      	|
| 		       		   |  op_queue_max_ops   			| 队列中最大的操作数      		|
| 		       		   |  op_queue_max_ops   			| 队列队中的操作数      		|
| 		       		   |  ops   						| 操作数      				|
| 		       		   |  op_queue_max_bytes   			| 操作队列最大bytes数      	|
| 		       		   |  op_queue_bytes   				| 操作队列bytes数      		|
| 		       		   |  bytes   						| 写入存储的数据     			|
| 		       		   |  apply_latency.avgcount   		| 申请延迟队列平均数      		|
| 		       		   |  apply_latency.sum   			| 申请延迟队列总数      		|
| 		       		   |  apply_latency.avgtime   		| 申请延迟队列平均时间      	|
| 		       		   |  queue_transaction_latency_avg.avgcount   		| 存储操作等待队列平均数      	|
| 		       		   |  queue_transaction_latency_avg.sum   		| 存储操作等待队列总数      	|
| 		       		   |  queue_transaction_latency_avg.avgtime   		| 存储操作等待队列平均数      	|


### 3. leveldb
| 监控类型   |      监控项      |  说明  |
|----------|:-------------:|:-------------:|
| perf dump leveldb  |  leveldb_get         				 | 获取的数量	    					|
|   				 |  leveldb_get_latency.avgcount         | 获取延迟队列里面的平均数量	    	|
| 					 |  leveldb_get_latency.sum         	 | 获取延迟队列里面的总数	    		|
| 					 |  leveldb_submit_latency.avgcount      | 提交延迟队列里面的平均数量	    	|
| 				     |  leveldb_submit_latency.sum         	 | 提交延迟队列里面的总数	    		|
| 					 |  leveldb_submit_sync_latency.avgcount | 提交同步延迟队列里面的平均数量	    |
| 					 |  leveldb_submit_sync_latency.sum      | 提交同步延迟队列里面的总数	    	|
| 					 |  leveldb_compact      	 		     | 压缩	    					    |
| 					 |  leveldb_compact_range   			 | 压缩范围	    					|
| 					 |  leveldb_compact_queue_merge      	 | 压缩合并队列	    				|
| 					 |  leveldb_compact_queue_len      	 	 | 压缩队列长度	    				|



	
### 4. objecter
| 监控类型   |      监控项      |  说明  |
|----------|:-------------:|:-------------:|
| perf dump objecter  |  op_active         				 	  | 主动操作数	    					|
|   				  |  op_laggy         					  | 消极操作数	    					|
|   				  |  op_send         					  | 发送操作数	    					|
|   				  |  op_send_bytes         				  | 发送操作bytes	    					|
|   				  |  op_resend         				  	  | 重操作数    							|
|   				  |  op_reply         				  	  | 回复操作数	    					|
|   				  |  op         				  		  | 操作数	    						|
|   				  |  op_r         				  		  | 读操作数    							|
|   				  |  op_w         				  		  | 写操作数    							|
|   				  |  op_rmw         				  	  | 读写修改操作数    					|
|   				  |  op_pg         				  		  | PG操作数    							|
|   				  |  osdop_stat         				  | 操作状态    							|
|   				  |  osdop_create         				  | 创建对象操作    						|
|   				  |  osdop_read         				  | 读操作    							|
|   				  |  osdop_write         				  | 写操作    							|
|   				  |  osdop_writefull         			  | 写满对象操作    						|
|   				  |  osdop_writesame         			  | 写相同的对象操作    					|
|   				  |  osdop_append         				  | 追加操作    							|
|   				  |  osdop_zero         				  | 设置对象0操作    						|
|   				  |  osdop_truncate         			  | 截断对象操作    						|
|   				  |  osdop_delete         				  | 删除对象操作    						|
|   				  |  osdop_mapext         				  | 映射范围操作    						|
|   				  |  osdop_sparse_read         			  | 稀少读操作    						|
|   				  |  osdop_clonerange         			  | 克隆范围操作    						|
|   				  |  osdop_getxattr         			  | 获取xattr操作    						|
|   				  |  osdop_setxattr         			  | 设置xattr操作    						|
|   				  |  osdop_cmpxattr         			  | 比较xattr操作    						|
|   				  |  osdop_rmxattr         			  	  | 移除xattr操作    						|
|   				  |  osdop_resetxattrs         			  | 重置xattr操作    						|
|   				  |  osdop_tmap_up         			  	  | tmap更新操作    						|
|   				  |  osdop_tmap_put         			  | tmap推送操作    						|
|   				  |  osdop_tmap_get         			  | tmap获取操作    						|
|   				  |  osdop_call         			  	  | 调用执行操作    						|
|   				  |  osdop_watch         			  	  | 监控对象操作    						|
|   				  |  osdop_notify         			  	  | 对象操作通知    						|
|   				  |  osdop_src_cmpxattr         		  | 多个操作扩展属性    					|
|   				  |  osdop_pgls         		  		  | pg对象操作   							|
|   				  |  osdop_pgls_filter         		  	  | pg过滤对象操作    					|
|   				  |  osdop_other         		  		  | 其他操作    							|
|   				  |  linger_active         		  		  | 主动延迟操作    						|
|   				  |  linger_send         		  		  | 延迟发送操作    						|
|   				  |  linger_resend         		  		  | 延迟重新发送    						|
|   				  |  linger_ping         		  		  | 延迟ping操作    						|
|   				  |  poolop_active         		  		  | 主动池操作    						|
|   				  |  poolop_send         		  		  | 发送池操作   							|
|   				  |  poolop_resend         		  		  | 重新发送池操作	   							|
|   				  |  poolstat_active         		  	  | 主动获取池子统计操作   							|
|   				  |  poolstat_send         		  		  | 发送池子统计操作   							|
|   				  |  poolstat_resend         		  	  | 重新发送池子统计操作   							|
|   				  |  statfs_active         		  		  | fs状态操作   							|
|   				  |  statfs_send         		  		  | 发送fs状态   							|
|   				  |  statfs_resend         		  		  | 重新发送fs状态   							|
|   				  |  command_active         		  	  | 活动的命令   							|
|   				  |  command_send         		  		  | 发送指令  							|
|   				  |  command_resend         		  	  | 重新发送指令  							|
|   				  |  map_epoch         		  	  		  | OSD map epoch  							|
|   				  |  map_full         		  	  		  | 接收满的OSD map  							|
|   				  |  map_inc         		  	  		  | 接收到增量OSD map  							|
|   				  |  osd_sessions         		  	  	  | osd 会话  							|
|   				  |  osd_session_open         		  	  	  | 打开osd会话  							|
|   				  |  osd_session_close         		  	  	  | 关闭osd会话  							|
|   				  |  osd_laggy         		  	  	  	 | 缓慢的osd会话  							|
|   				  |  omap_wr         		  	  	  	 | osd map读写操作  							|
|   				  |  omap_rd         		  	  	  	 | osd map读操作  							|
|   				  |  omap_del         		  	  	  	 | osd map删除操作  							|










		


	


		


		


		




