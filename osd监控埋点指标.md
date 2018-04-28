### 1. WBThrottle

| 监控类型   |      监控项      |  说明 |
|----------|:-------------:|------:|
| perf dump WBThrottle |  bytes_dirtied         | 脏数据大小     |
|                      |  bytes_wb              | 写入数据大小   |
|                      |  ios_dirtied           | 脏数据操作	    |
|                      |  ios_wb                | 写操作        |
|                      |  inodes_dirtied        | 等待写入的条目        |
|                      |  inodes_wb             | 写记录        |

### 2. filestore
| 监控类型   |      监控项      |  说明 |
|----------|:-------------:|------:|
| perf dump filestore  |  journal_queue_max_ops         | 日志队列中的最大操作	    |
| 		       |  journal_queue_ops         	| 日志队列中的操作大小	    |
| 		       |  journal_ops         		| 日志请求数	    		|
| 		       |  journal_bytes         	| 日志大小	    		|
| 		       |  journal_latency.avgcount      | 日志等待队列平均大小	    |
| 		       |  journal_latency.sum      	| 日志等待队列总大小	    |
| 		       |  journal_latency.avgtime       | 日志等待队列平均时间	    |
| 		       |  journal_wr      		| 日志读写io	    	|
| 		       |  journal_wr_bytes.avgcount     | 日志读写大小的队列平均数量	  |
| 		       |  journal_wr_bytes.sum      	| 日志读写大小的队列总数	   |
| 		       |  journal_full      		| 日志写满	   		|
| 		       |  committing     		| 正在提交数量	       |
| 		       |  commitcycle_interval.avgcount | 提交之间的间隔队列平均数量	  |
| 		       |  commitcycle_interval.sum     	| 提交之间的间隔队列总数	    |
| 		       |  commitcycle_interval.avgtime  | 提交之间的间隔队列平均时间	  |
| 		       |  commitcycle_latency.avgcount  | 提交延迟队列平均数量	    |
| 		       |  commitcycle_latency.sum     	| 提交延迟队列总数	      |






				



	


		


	


		


		


		




