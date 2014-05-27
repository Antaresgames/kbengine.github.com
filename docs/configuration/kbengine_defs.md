---
layout: docs
title: Server Configuration · Docs · KBEngine
tab: docs
docsitem: configuration-kbengine-defs
---

Server Configuration(kbe/res/server/kbengine_defs.xml)
====================

If you need to change the configuration of the engine, as long as the inheritance ({newproject}[/res/server/kbengine.xml]) 
this configuration file and overwrite the corresponding section kbengine_defs.xml, but doing so will not damage the engine settings, 
there is no conflict in the development along with updated engine or multiple projects.


-----------------------------------------------------
###Detailed configuration:


	<root>
		<gameUpdateHertz> 10 </gameUpdateHertz>
		<bitsPerSecondToClient> 20000 </bitsPerSecondToClient> <!-- Type: Integer -->
		
		<!-- Non-0 is not optimized, Force all packets contain length information, Some convenient docking protocol client.
		What of data packet length information does not? All packages can be calculated in advance 
		and never change the size of the package with no length information. -->
		<packetAlwaysContainLength>0</packetAlwaysContainLength>
		
		<!-- Whether the contents of the log output packet
			debug_type:
				0: Not output
				1: 16 hexadecimal output
				2: Stringstream output
				3: 10 hexadecimal output
			use_logfile:
				Whether the use of other log files ? such as:
				appname_packetlogs.log
			disable_msgs:
				关闭某些包的输出
		-->
		<trace_packet>
			<debug_type>0</debug_type>
			<use_logfile>false</use_logfile>
			<disables>
				<item>Baseappmgr::updateBaseapp</item>
				<item>Cellapp::onUpdateDataFromClient</item>
				<item>Baseapp::onUpdateDataFromClient</item>
				<item>Baseapp::forwardMessageToClientFromCellapp</item>
				<item>Client::onUpdateVolatileData</item>
				<item>Client::onUpdateData</item>
				<item>Client::onUpdateBasePos</item>
				<item>Client::onUpdateData_xz</item>
				<item>Client::onUpdateData_xyz</item>
				<item>Client::onUpdateData_y</item>
				<item>Client::onUpdateData_r</item>
				<item>Client::onUpdateData_p</item>
				<item>Client::onUpdateData_ypr</item>
				<item>Client::onUpdateData_yp</item>
				<item>Client::onUpdateData_yr</item>
				<item>Client::onUpdateData_pr</item>
				<item>Client::onUpdateData_xz_y</item>
				<item>Client::onUpdateData_xz_p</item>
				<item>Client::onUpdateData_xz_r</item>
				<item>Client::onUpdateData_xz_yr</item>
				<item>Client::onUpdateData_xz_yp</item>
				<item>Client::onUpdateData_xz_pr</item>
				<item>Client::onUpdateData_xz_ypr</item>
				<item>Client::onUpdateData_xyz_y</item>
				<item>Client::onUpdateData_xyz_p</item>
				<item>Client::onUpdateData_xyz_r</item>
				<item>Client::onUpdateData_xyz_yr</item>
				<item>Client::onUpdateData_xyz_yp</item>
				<item>Client::onUpdateData_xyz_pr</item>
				<item>Client::onUpdateData_xyz_ypr</item>
			</disables>
		</trace_packet>
		
		<!-- 是否输出entity的创建， 脚本获取属性， 初始化属性等调试信息， 以及def信息 -->
		<debugEntity>0</debugEntity>

		<!-- apps发布状态, 可在脚本中获取该值
			Type: Integer8
			0 : debug
			1 : release
			其他自定义
		-->
		<app_publish>0</app_publish>
		
		<cellapps> 1 </cellapps>
		<baseapps> 1 </baseapps>
		
		<channelCommon> 
			<!-- 通道最后一次通信时间超过此时间则被认定为超时通道， 服务器将踢出该通道 -->
			<timeout> 
				<internal> 60.0 </internal>
				<external> 60.0 </external>
			</timeout>

			<!-- 通道send数据重试时间次数 -->
			<resend> 
				<internal> 
					<interval> 10 </interval>					<!-- 毫秒 -->
					<retries> 0 </retries>						<!-- 重试次数, 0无限 -->
				</internal>
				<external>
					<interval> 10 </interval>					<!-- 毫秒 -->
					<retries> 3 </retries>						<!-- 重试次数, 0无限 -->
				</external>
			</resend>
			
			<!-- socket读写缓冲大小 -->
			<readBufferSize> 
				<internal>	16777216	</internal> 			<!-- 16M -->
				<external>	0			</external>				<!-- 系统默认 -->
			</readBufferSize>
			<writeBufferSize> 
				<internal>	16777216	</internal>				<!-- 16M -->
				<external>	0			</external>				<!-- 系统默认 -->
			</writeBufferSize>
			
			<!-- 一个tick内channel接收窗口溢出值 0无限制-->
			<receiveWindowOverflow>
				<messages>
					<critical>	32			</critical>
					<internal>	65535		</internal>
					<external>	256			</external>
				</messages>
				<bytes>
					<internal>	0			</internal>
					<external>	65535		</external>
				</bytes>
			</receiveWindowOverflow>
			
			<!-- 加密通信，只对外部通道
				可选加密:
					0: 无加密
					1: blowfish
					2: rsa (res\key\kbengine_private.key)
			 -->
			<encrypt_type> 1 </encrypt_type>
		</channelCommon> 
		
		<!-- 关服倒计时(秒) -->
		<shutdown_time> 60.0 </shutdown_time>
		<!-- 关服等待任务结束tick(秒) -->
		<shutdown_waittick> 1.0 </shutdown_waittick>
		
		<!-- callback默认超时时间(秒) -->
		<callback_timeout> 300.0 </callback_timeout>
		
		<thread_pool>
			<!-- 默认超时时间(秒) -->
			<timeout> 300.0 </timeout>
			
			<init_create> 1 </init_create>
			<pre_create> 2 </pre_create>
			<max_create> 8 </max_create>
		</thread_pool>
		
		<!-- email 服务 -->
		<email_service_config>server/email_service.xml</email_service_config>
			
		<billingSystem>
			<!-- normal: 普通, thirdparty: 第三方此时需要提供ip地址 -->
			<accountType> normal </accountType>
			<chargeType> normal </chargeType>
			
			<!-- 进程地址 -->
			<host> localhost </host>
			<port> 30099 	</port>
			
			<!-- 订单超时(secs) -->
			<orders_timeout> 3600 	</orders_timeout>
			
			<!-- 第三方运营账号服务接口 如: www.google.com, 当type是thirdparty时有效 -->
			<thirdpartyAccountService_addr></thirdpartyAccountService_addr>
			<thirdpartyAccountService_port> 80 </thirdpartyAccountService_port>
			
			<!-- 第三方运营充值服务接口 如: www.google.com, 当type是thirdparty时有效 -->
			<thirdpartyChargeService_addr></thirdpartyChargeService_addr>
			<thirdpartyChargeService_port> 80 </thirdpartyChargeService_port>
			
			<!-- 第三方运营回调端口，这个通道是一个未定义的协议通道， 相关协议需要开发者根据需要自己解析-->
			<thirdpartyService_cbport> 30040 </thirdpartyService_cbport>
			
			<!-- listen监听队列最大值 -->
			<SOMAXCONN> 128 </SOMAXCONN>
		</billingSystem>
		
		<dbmgr>
			<dbAccountEntityScriptType>	Account	</dbAccountEntityScriptType>
			
			<allowEmptyDigest> false </allowEmptyDigest>					<!-- Type: Boolean -->
			
			<internalInterface>  </internalInterface>
			
			<!-- 新账号默认标记(可叠加， 填写时按十进制格式) 
				常规标记 = 0x00000000
				锁定标记 = 0x000000001
				未激活标记 = 0x000000002
			-->
			<accountDefaultFlags> 0 </accountDefaultFlags>					<!-- Type: Integer -->
			<!-- 新账号默认过期时间(秒, 引擎会加上当前时间) -->
			<accountDefaultDeadline> 0 </accountDefaultDeadline>			<!-- Type: Integer -->
			
			<type> mysql </type>											<!-- Type: String -->
			<host> localhost </host>										<!-- Type: String -->
			<port> 0 </port>												<!-- Type: Integer -->
			<auth>  
				<username> kbe </username>									<!-- Type: String -->
				<password> kbe </password>									<!-- Type: String -->
				<!-- 为true则表示password是加密(rsa)的, 可防止明文配置 -->
				<encrypt>true</encrypt>
			</auth>
			<databaseName> kbe </databaseName> 								<!-- Type: String -->
			<numConnections> 5 </numConnections>							<!-- Type: Integer -->
			
			<unicodeString>
				<characterSet> utf8 </characterSet> 						<!-- Type: String -->
				<collation> utf8_bin </collation> 							<!-- Type: String -->
			</unicodeString>
			
			<!-- 登录合法时游戏数据库找不到游戏账号则自动创建 -->
			<notFoundAccountAutoCreate> true </notFoundAccountAutoCreate>
		</dbmgr>
		
		<cellapp>
			<entryScriptFile> kbengine </entryScriptFile>
			
			<defaultAoIRadius>			
				<radius> 80.0 </radius>
				<hysteresisArea> 5.0 </hysteresisArea>
			</defaultAoIRadius>
			
			<!-- 优化EntityID，aoi范围内小于255个, EntityID传输到client时使用1字节别名ID -->
			<aliasEntityID> true </aliasEntityID>
			
			<!-- 优化entity属性和方法广播时占用的带宽，entity客户端属性或者客户端不超过255个时， 
				方法uid和属性uid传输到client时使用1字节别名ID -->
			<entitydefAliasID>true</entitydefAliasID>
		
			<internalInterface>  </internalInterface>
			
			<ids>
				<criticallyLowSize> 500 </criticallyLowSize>				<!-- Type: Integer -->
			</ids>
			
			<profiles>
				<!-- 如果设置为true，引擎启动时就会开始记录相关profile信息， 退出时导出一份记录 -->
				<cprofile> false </cprofile>
				<pyprofile> false </pyprofile>
				<eventprofile> false </eventprofile>
				<mercuryprofile> false </mercuryprofile>
			</profiles>
			
			<ghostDistance> 500.0 </ghostDistance>
			<ghostingMaxPerCheck> 64 </ghostingMaxPerCheck> <!-- Type: Integer -->
			<ghostUpdateHertz> 50 </ghostUpdateHertz> <!-- Type: Integer -->
			
			<!-- 是否使用坐标系统 如果为false， aoi、trap、 move等功能将不再维护 -->
			<coordinate_system> 
				<enable> true </enable>
				
				<!-- 范围管理器是管理Y轴， 注：有y轴则aoi、trap等功能有了高度， 
					但y轴的管理会带来一定的消耗 -->
				<rangemgr_y> false </rangemgr_y>
			</coordinate_system>
			
			<!-- 如果被占用则向后尝试50001.. -->
			<telnet_service>
				<port> 50000 </port>
				<password> kbe </password>
				<!-- 命令默认层 -->
				<default_layer> python </default_layer>
			</telnet_service>
			
			<shutdown>
				<!-- 每秒销毁有base的entity -->
				<perSecsDestroyEntitySize> 15 </perSecsDestroyEntitySize>
			</shutdown>
			
			<witness>
				<!-- 观察者 timeout超时(秒) Integer-->
				<timeout> 15 </timeout>
			</witness>
		</cellapp>
		
		<baseapp>
			<entryScriptFile> kbengine </entryScriptFile>
			
			<internalInterface>  </internalInterface>
			<externalInterface>  </externalInterface>						<!-- Type: String -->
			<externalPorts_min> 20015 </externalPorts_min>					<!-- Type: Integer -->
			<externalPorts_max> 20019 </externalPorts_max>					<!-- Type: Integer -->
			
			<archivePeriod> 100 </archivePeriod> 							<!-- Type: Float -->
			<backupPeriod> 10 </backupPeriod>								<!-- Type: Float -->
			<backUpUndefinedProperties> 0 </backUpUndefinedProperties>		<!-- Type: Boolean -->
			
			<loadSmoothingBias> 0.01 </loadSmoothingBias>
			
			<downloadStreaming>
				<bitsPerSecondTotal> 1000000 </bitsPerSecondTotal>			<!-- Type: Int -->
				<bitsPerSecondPerClient> 100000 </bitsPerSecondPerClient>	<!-- Type: Int -->
			</downloadStreaming>
			
			<ids>
				<criticallyLowSize> 500 </criticallyLowSize>				<!-- Type: Integer -->
			</ids>
			
			<!-- entity restore每tick数量 -->
			<entityRestoreSize> 32 </entityRestoreSize>
			
			<profiles>
				<!-- 如果设置为true，引擎启动时就会开始记录相关profile信息， 退出时导出一份记录 -->
				<cprofile> false </cprofile>
				<pyprofile> false </pyprofile>
				<eventprofile> false </eventprofile>
				<mercuryprofile> false </mercuryprofile>
			</profiles>
			
			<!-- listen监听队列最大值 -->
			<SOMAXCONN> 128 </SOMAXCONN>
			
			<!-- 如果被占用则向后尝试40001.. -->
			<telnet_service>
				<port> 40000 </port>
				<password> kbe </password>
				<!-- 命令默认层 -->
				<default_layer> python </default_layer>
			</telnet_service>
			
			<shutdown>
				<!-- 每秒销毁base数量 -->
				<perSecsDestroyEntitySize> 15 </perSecsDestroyEntitySize>
			</shutdown>
			
			<respool>
				<!-- 缓冲区大小也等于资源大小， 在这个KB大小范围以内的资源才可以进入资源池 -->
				<buffer_size> 1024 </buffer_size>
				<!-- 资源池中的资源超过这个时间未被访问则销毁(秒) -->
				<timeout> 600 </timeout>
				<!-- 资源池检查tick(秒) -->
				<checktick>60</checktick>
			</respool>
		</baseapp>
		
		<cellappmgr>
			<internalInterface>  </internalInterface>
		</cellappmgr>
		
		<baseappmgr>
			<internalInterface>  </internalInterface>
		</baseappmgr>
		
		<loginapp>
			<internalInterface>  </internalInterface>
			<externalInterface>  </externalInterface>						<!-- Type: String -->
			<externalPorts_min> 20013 </externalPorts_min>					<!-- Type: Integer -->
			<externalPorts_max> 0 </externalPorts_max>						<!-- Type: Integer -->
			
			<!-- 加密登录信息
				可选加密:
					0: 无加密
					1: blowfish
					2: rsa (res\key\kbengine_private.key)
			 -->
			<encrypt_login> 2 </encrypt_login>
			
			<!-- listen监听队列最大值 -->
			<SOMAXCONN> 128 </SOMAXCONN>
			
			<!-- 
				1: 普通账号
				2: email账号(需要激活)
				3: 智能账号(自动识别email， 普通号码等) 
			-->
			<account_type> 3 </account_type>
			
			<!-- 用户http回调接口，处理认证、密码重置等 -->
			<http_cbhost> localhost </http_cbhost>
			<http_cbport> 21103 </http_cbport>
		</loginapp>		
		
		<kbmachine>
			<externalPorts_min> 20099 </externalPorts_min>					<!-- Type: Integer -->
			<externalPorts_max> 0 </externalPorts_max>						<!-- Type: Integer -->
		</kbmachine>
		
		<kbcenter>
			<internalInterface>  </internalInterface>
		</kbcenter>
		
		<bots>
			<internalInterface>  </internalInterface>
			<host> localhost </host>										<!-- Type: String -->
			<port> 20013 </port>											<!-- Type: Integer -->
			
			<!-- 默认启动进程后自动添加这么多个bots 
				totalcount： 添加总数量
				ticktime： 每次添加所用时间(s)
				tickcount： 每次添加数量
			-->
			<defaultAddBots> 
				<totalcount> 10  </totalcount> <!-- Type: Integer -->
				<ticktime> 0.1  </ticktime> <!-- Type: Float -->
				<tickcount> 2  </tickcount> <!-- Type: Integer -->
			</defaultAddBots>							
		</bots>
		
		<messagelog>
			<internalInterface>  </internalInterface>
		</messagelog>
		
		<resourcemgr>
			<internalInterface>  </internalInterface>
			
			<downloadStreaming>
				<bitsPerSecondTotal> 1000000 </bitsPerSecondTotal>			<!-- Type: Int -->
				<bitsPerSecondPerClient> 100000 </bitsPerSecondPerClient>	<!-- Type: Int -->
			</downloadStreaming>
			
			<!-- listen监听队列最大值 -->
			<SOMAXCONN> 128 </SOMAXCONN>
			
			<respool>
				<!-- 缓冲区大小也等于资源大小， 在这个KB大小范围以内的资源才可以进入资源池 -->
				<buffer_size> 1024 </buffer_size>
				<!-- 资源池中的资源超过这个时间未被访问则销毁(秒) -->
				<timeout> 600 </timeout>
				<!-- 资源池检查tick(秒) -->
				<checktick>60</checktick>
			</respool>
		</resourcemgr>
	</root>


[kbengine.xml]: {{ site.baseurl }}/docs/configuration/kbengine.html