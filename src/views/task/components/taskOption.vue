/*
 * @Author: lk 
 * @Date: 2020-02-27 10:33:39 
 * @Last Modified by: lk
 * @Last Modified time: 2020-03-04 14:45:48
 * @Description:  项目操作
 */
 <template>
     <div class="task-option">
         <!-- 项目基本信息 -->
         <h4 class="model-title">
             项目基本信息
         </h4>
         <div class="model-container">
             <el-form :rules="baseRules"
                inline
                 ref="baseForm"
                 :model="baseItem"
                 label-position="left"
                 label-width="83px">
                <el-form-item prop="protName" class="extend-length"
                                label="项目名称:">
                    <el-input class="form-input"
                            v-model.trim="baseItem.protName"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protNameText" class="extend-length"
                                label="项目描述:">
                    <el-input class="form-input"
                            v-model.trim="baseItem.protNameText"
                              type="textarea"
                            :rows="3"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protAuthor"
                                label="作者:">
                    <el-input class="form-input" style="width:255px;"
                            v-model.trim="baseItem.protAuthor"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protStart"
                              label="策略优先:" >
                    <el-radio v-model="baseItem.protStart" label="0">深度优先</el-radio>
                    <el-radio v-model="baseItem.protStart" label="1">广度优先</el-radio>
                </el-form-item>
                <el-form-item prop="protWebUri" class="extend-length"
                                label="起始地址:">
                    <el-input class="form-input"
                            v-model.trim="baseItem.protWebUri"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protWebName" class="extend-length"
                                label="网站名称:">
                    <el-input class="form-input"
                            v-model.trim="baseItem.protWebName"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protWebNameText" class="extend-length"
                                label="网站描述:">
                    <el-input class="form-input"
                            v-model.trim="baseItem.protWebNameText"
                            type="textarea"
                            :rows="3"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="confSysList"
                                label="系统环境:" class="extend-length-href">
                    <ul class="result-list" v-if="baseItem.confSysList && baseItem.confSysList.length>0">
                        <li v-for="item in baseItem.confSysList" :key="item.sysId">
                            {{item.sysName}}
                            <div class="result-option">
                                <i class="el-icon-edit"  @click="updateOptions('confSysList','sysId',item.sysId)"></i>
                                <i class="el-icon-delete" @click="deleteOptions('confSysList','sysId',item.sysId)"></i>
                            </div>
                        </li>
                    </ul>
                    <el-button class="add-result" size="mini" @click="addOptions('confSysList')"><i class="el-icon-plus"></i></el-button>
                </el-form-item>
                <el-form-item prop="confAgntList"
                                label="代理服务:" class="extend-length-href">
                    <ul class="result-list"  v-if="baseItem.confAgntList && baseItem.confAgntList.length>0">
                        <li v-for="item in baseItem.confAgntList" :key="item.agntId">
                            {{item.agntName}}
                            <div class="result-option">
                                <i class="el-icon-edit" @click="updateOptions('confAgntList','agntId',item.agntId)"></i>
                                <i class="el-icon-delete" @click="deleteOptions('confAgntList','agntId',item.agntId)"></i>
                            </div>
                        </li>
                    </ul>
                    <el-button class="add-result" size="mini" @click="addOptions('confAgntList')"><i class="el-icon-plus"></i></el-button>
                </el-form-item>
                <el-form-item prop="protState" class="extend-length"
                               label="项目状态:">
                    <el-radio v-model="baseItem.protState" label="0">删除</el-radio>
                    <el-radio v-model="baseItem.protState" label="1">停用</el-radio>
                    <el-radio v-model="baseItem.protState" label="2">启用</el-radio>
                </el-form-item>
            </el-form>
            <p class="model-status-row model-status-row-button">
               <el-button @click="save">保存</el-button>
               <el-button @click="reset">重置</el-button>
             </p>
         </div>
         <!-- 项目基本信息end -->
         <el-form :rules="ortherRules"
                inline
                 ref="otherForm"
                 :model="otherItem"
                 >
         <!-- 线程控制 -->
         <h4 class="model-title">
             线程控制
         </h4>
         <div class="model-container">
                <el-form-item prop="protMaxT" 
                                >
                    最大同时发起 
                    <el-input-number style="width:150px;" v-model="otherItem.protMaxT"  :min="1"  ></el-input-number>
                    个线程来下载
                </el-form-item>
                <el-form-item prop="protMinT" 
                                >
                    最小同时发起 
                    <el-input-number style="width:150px;" v-model="otherItem.protMinT"  :min="1"  ></el-input-number>
                    个线程来下载
                </el-form-item>
                <el-form-item prop="protDelT" class="extend-length"
                                >
                    如果经过 
                    <el-input-number style="width:150px;" v-model="otherItem.protDelT"  :min="1" ></el-input-number>
                    秒没有使用则放弃该线程
                </el-form-item>
         </div>
         <!-- 线程控制end -->
         <!-- 下载选项 -->
         <h4 class="model-title">
             下载选项
         </h4>
         <div class="model-container">
                <el-form-item prop="protDolPath" class="extend-length long-length"
                                label="下载文件储存根路径:">
                    <el-input class="form-input"
                            v-model.trim="otherItem.protDolPath"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protDolPathName" class="extend-length long-length"
                                label="子文件目录名:">
                    <el-radio v-model="otherItem.protDolPathName" label="0">日期</el-radio>
                    <el-radio v-model="otherItem.protDolPathName" label="1">原网站陌路结构</el-radio>
                </el-form-item>
                <el-form-item prop="protDolFileName" class="extend-length long-length"
                                label="文件命名方案:">
                    <el-radio v-model="otherItem.protDolFileName" label="0">原名称</el-radio>
                    <el-radio v-model="otherItem.protDolFileName" label="1">MD5</el-radio>
                    <el-radio v-model="otherItem.protDolFileName" label="2">毫秒数</el-radio>
                </el-form-item>
                <el-form-item  class="extend-length"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolSaveHtml">总是保存原网站的HTML</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolUseMsie">使用MSIE兼容文件名（如有需要使用.htm为HTML文件的附加命名）</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolChar">如果必要，添加HTML和字符集标签</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolAdd">给原始URL添加元标签和下载的日期/时间</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolLocal">将下载的文件链接本地化</el-checkbox><br>
                </el-form-item>
                <el-form-item prop="protDolLength" class="extend-length long-length"
                                >
                    <el-radio v-model="otherItem.protDolLengthStart" label="0">下载所有文件，除了那些超过
                        <el-input-number style="width:150px;" v-model="otherItem.protDolLength"  :min="0" ></el-input-number>
                        KB的文件（0=无限制）
                    </el-radio><br/>
                    <el-radio v-model="otherItem.protDolLengthStart" label="2">仅下载下列类型和大小的文件（以KB计算）</el-radio>
                    <el-table
                      v-if="otherItem.protDolLengthStart === '2'"
                        :data="otherItem.confDonList"
                        style="width: 100%;margin-left:15px;">
                        <el-table-column
                            prop="protDonType"
                            label="类别">
                        </el-table-column>
                        <el-table-column
                            prop="protDonFileType"
                            label="文件类型">
                        </el-table-column>
                        <el-table-column
                            prop="protDonMax"
                            label="最大">
                        </el-table-column>
                        <el-table-column
                            prop="protDonMin"
                            label="最小">
                        </el-table-column>
                        <el-table-column
                            label="操作">
                            <template slot="header">
                                操作 <i class="el-icon-plus" @click="addOptions('confDonList')"></i>
                            </template>
                            <template slot-scope="scope">
                                <i class="el-icon-edit" @click="confDonListUpdate(scope)"></i>
                                <i class="el-icon-delete" @click="confDonListDelete(scope)"></i>
                            </template>
                        </el-table-column>
                        </el-table>
                </el-form-item>
                <el-form-item  class="extend-length" style="padding-left:30px;"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolFile">下载内嵌文件（例如：图像和音频文件）</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolBackImg">下载背景图像</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolJava">下载Java程序</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protDolGroup">仅下载文件名称（即：只生成站点地图，不保存文件）</el-checkbox><br>
                </el-form-item>
         </div>
         <!-- 下载选项end -->
         <!-- 排除选项 -->
         <h4 class="model-title">
             排除选项
         </h4>
         <div class="model-container">
                <el-form-item prop="protFiltCharUl" class="extend-length"
                                >
                    <div class="f-left">注意：排除标准如果与项目设定冲突，则有优先权</div>
                    <el-checkbox  true-label="1" false-label="0" class="f-right" style="width:150px;" v-model="otherItem.protFiltCharUl">匹配大小写</el-checkbox>
                </el-form-item>
                <el-form-item prop="protFiltAllChar" class="extend-length"
                                >
                    <div class="f-left" style="width:550px;line-height: 23px;">不保存信息网页，或与信息网页链接的文件。除非这些网页中包含以下关键字（你可
                以使用*和？做替换字符；请使用分行分割关键字</div>
                    <el-checkbox  true-label="1" false-label="0" class="f-right" style="width:150px;" v-model="otherItem.protFiltAllChar">匹配整个字符串</el-checkbox>
                </el-form-item>
                <el-form-item prop="protFiltChar" class="extend-length whole-length">
                    <el-input 
                            v-model.trim="otherItem.protFiltChar"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protFiltFileName" class="extend-length whole-length">
                    <div>不要下载任何符合以下DOS扩展名的文件（请以分号来分割扩展名，例如*.avi;*.jpg)</div>
                    <el-input 
                            v-model.trim="otherItem.protFiltFileName"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protFiltId" class="extend-length whole-length">
                    <div style="width:550px;line-height: 23px;">以下列部分网址为启始的连接不要追踪，也不要收集他们的文件。（使用“http://”
                        代表http忘记“ftp://”代表ftp地址。你也可以使用*和？替换字符。每个网址各占一行。</div>
                    <ul class="result-list" v-if="otherItem.confFiltList && otherItem.confFiltList.length>0">
                        <li v-for="item in otherItem.confFiltList" :key="item.protFiltId">
                            {{item.protFilt}}
                            <div class="result-option">
                                <i class="el-icon-edit" @click="updateOptions('confFiltList','protFiltId',item.protFiltId)"></i>
                                <i class="el-icon-delete" @click="deleteOtherOptions('confFiltList','protFiltId',item.protFiltId)"></i>
                            </div>
                        </li>
                    </ul>
                    <el-button class="add-result" size="mini"  @click="addOptions('confFiltList')"><i class="el-icon-plus"></i></el-button>
                </el-form-item>
         </div>
         <!-- 排除选项end -->
         <!-- 网络礼节 -->
         <h4 class="model-title">
             网络礼节
         </h4>
         <div class="model-container">
                <el-form-item class="extend-length"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protCoryField">启动域分散搜索（将连接请求分散给不同的服务器，避免缓慢的服务器造成网络拥塞）。</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protCorySerd">启动服务器过载保护（自动降低对于速度缓慢之服务器在同一时间内的数据请求次数）。</el-checkbox><br>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protCoryRemoves">遵循自动排除标准（避免爬虫 进入站点管理员标记为无权存取的区域）。</el-checkbox><br>
                </el-form-item>
                <el-form-item prop="protCoryTime"
                                >
                    等待 
                    <el-input-number style="width:150px;" v-model="otherItem.protCoryTime"  :min="0" :max="1000" ></el-input-number>
                    秒,当请求
                    <el-radio v-model="otherItem.protCorySystem" label="0">较慢的服务器</el-radio>
                    <el-radio v-model="otherItem.protCorySystem" label="1">所有服务器</el-radio>
                </el-form-item>
                <el-form-item prop="protCoryUseField" class="extend-length long-length"
                                label="使用此域提交所有请求:">
                    <el-input class="form-input"
                            v-model.trim="otherItem.protCoryUseField"
                            clearable></el-input>
                </el-form-item>
                <el-form-item prop="protCoryUser" 
                                label="程序标识:">
                    <el-radio v-model="otherItem.protCoryType" label="0">匿名</el-radio><br/>
                    <el-radio v-model="otherItem.protCoryType" label="1">告知对方是爬虫</el-radio><br/>
                    <el-radio v-model="otherItem.protCoryType" label="2">假装是 Microsoft Internet Explorer</el-radio><br/>
                    <el-radio v-model="otherItem.protCoryType" label="3">假装是 Chrome</el-radio><br/>
                    <el-radio v-model="otherItem.protCoryType" label="4">使用这个身份</el-radio>
                    <el-input class="form-input" style="width:255px;"
                            v-model.trim="otherItem.protCoryUser"
                            clearable></el-input>
                </el-form-item>
         </div>
         <!-- 网络礼节end -->
         <!-- 探测设置 -->
         <h4 class="model-title">
             探测设置
         </h4>
         <div class="model-container">
                <el-form-item prop="protSearchPic" class="extend-length"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protSearchPic">
                    </el-checkbox>
                        探测服务器端的映射图像（每隔
                    <el-input-number style="width:150px;" v-model="otherItem.protSearchPicNum"  :min="0" :max="30" ></el-input-number>
                        像素）
                </el-form-item>
                <el-form-item prop="protSearchFile"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protSearchFile">
                        探测框架文件</el-checkbox>
                </el-form-item>
                <el-form-item prop="protSearchJs"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protSearchJs">
                        处理脚本和事件代码</el-checkbox>
                </el-form-item>
                <el-form-item prop="protSearchTable"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protSearchTable">
                        尽可能探测表格</el-checkbox>
                </el-form-item>
                <el-form-item prop="protSearchCookies"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.protSearchCookies">
                        同意并返回Cookies</el-checkbox>
                </el-form-item>
                <br>
                <el-form-item prop="protSearchNum"
                                >
                        重复被拒绝请求
                        <el-input-number style="width:150px;" v-model="otherItem.protSearchNum"  :min="1" :max="10" ></el-input-number>
                        次
                </el-form-item>
                <el-form-item prop="protSearchFileNum"
                                >
                        没有完成的文件重复
                        <el-input-number style="width:150px;" v-model="otherItem.protSearchFileNum"  :min="1" :max="10"></el-input-number>
                        次后放弃。
                </el-form-item>
         </div>
         <!--探测设置end -->
         <!-- 更新设置 -->
         <h4 class="model-title">
             更新设置
         </h4>
         <div class="model-container">
                <el-form-item prop="portUpd" 
                                >
                    <el-radio v-model="otherItem.portUpd" label="0">更新好的文件</el-radio>
                    <el-radio v-model="otherItem.portUpd" label="1">更新坏的文件</el-radio>
                    <el-radio v-model="otherItem.portUpd" label="2">好坏文件都更新</el-radio>
                </el-form-item>
                <br>
                <el-form-item 
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portUpdHtml">更新HTML文件</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portUpdImg">更新服务器端映射图像</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portUpdFile">更新内嵌文件</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portUpdAll">更新所有其他文件</el-checkbox>
                </el-form-item> 
         </div>
         <!--更新设置end -->
         <!-- 队列设置 -->
         <h4 class="model-title">
             队列设置
         </h4>
         <div class="model-container">
                <el-form-item prop="portQueueRun"
                                >
                        待执行队列长度
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueRun"  :min="0"  ></el-input-number>
                        （0=无限制），
                </el-form-item>
                <el-form-item prop="portQueueRunTime"
                                >
                        每间隔
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueRunTime"  :min="60" :max="3600"></el-input-number>
                        秒自动同步到数据库中
                </el-form-item>
                <br>
                <el-form-item prop="portQueueErr"
                                >
                        错误队列长度
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueErr" :min="0" ></el-input-number>
                        （0=无限制），
                </el-form-item>
                <el-form-item prop="portQueueErrTime"
                                >
                        每间隔
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueErrTime"  :min="60" :max="3600"></el-input-number>
                        秒自动同步到数据库中
                </el-form-item>
                <br>
                <el-form-item prop="portQueueSus"
                                >
                        成功队列长度
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueSus"  :min="0"></el-input-number>
                </el-form-item>
                <el-form-item prop="portQueueSusTime"
                                >
                        （0=无限制），每间隔
                        <el-input-number style="width:150px;" v-model="otherItem.portQueueSusTime"  :min="60" :max="3600"></el-input-number>
                        秒自动同步到数据库中
                </el-form-item>
         </div>
         <!--队列设置end -->
         <!-- 解析设置 -->
         <h4 class="model-title">
             解析设置
         </h4>
         <div class="model-container">
                 <h4 style="height:20px;">文档设置</h4>
                <el-form-item prop="portAnysHtml"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysHtml">解析HTML</el-checkbox>
                </el-form-item>
                <el-form-item prop="portAnysJson"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysJson">解析JSON</el-checkbox>
                </el-form-item>
                <el-form-item prop="portAnysAll"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysAll">解析文件</el-checkbox>
                </el-form-item>
                <el-form-item prop="portAnysAllText"
                                >
                    <el-input class="form-input" style="width:255px;"
                            v-model.trim="otherItem.portAnysAllText"
                            clearable></el-input>
                </el-form-item>
                 <h4 style="height:30px;">内容设置</h4>
                <el-form-item prop="portAnysSource"
                                >
                    目标库:
                    <el-select class="form-input" style="width:255px;"
                            v-model="otherItem.portAnysSource"
                            @change="getTableSelect"
                            clearable>
                        <el-option v-for="(text,value) in selectOptions.source" :key="value" :value="value" :label="text"></el-option>        
                    </el-select>
                    <el-button icon="el-icon-plus" @click="addOptions('confSource')">创建</el-button>
                </el-form-item>
                <el-form-item prop="portAnysTable"
                                >
                    目标表名:
                    <el-select class="form-input" style="width:255px;"
                            v-model="otherItem.portAnysTable"
                            clearable>
                        <el-option v-for="(text,value) in selectOptions.table" :key="value" :value="value" :label="text"></el-option>        
                    </el-select>
                </el-form-item>
                <el-form-item class="extend-length"
                                >
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysTittle">政策法规主题</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysOrgText">发布单位</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysIssue">发文时间</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysEffective">生效时间</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysAnnul">废止时间</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysAbtName">关联政策法规</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysWeb">政策法规来源</el-checkbox>
                    <el-checkbox true-label="1" false-label="0" v-model="otherItem.portAnysOrder">其他</el-checkbox>
                    <el-table v-if="otherItem.portAnysOrder==='1'"
                        :data="otherItem.confAnysList"
                        style="width: 100%;margin-left:15px;">
                        <el-table-column
                          type="index"
                            label="编号">
                        </el-table-column>
                        <el-table-column
                            prop="protAnysTittle"
                            label="标题">
                        </el-table-column>
                        <el-table-column
                            prop="protAnysHtml"
                            label="HTML提取规则">
                        </el-table-column>
                        <el-table-column
                            prop="protAnysZz"
                            label="正则提取规则">
                        </el-table-column>
                        <el-table-column
                            prop="protAnysChar"
                            label="关键字">
                        </el-table-column>
                        <el-table-column
                            prop="protAnysIfsName"
                            label="接口调用">
                        </el-table-column>
                        <el-table-column
                            label="操作">
                            <template slot="header">
                                操作 <i class="el-icon-plus" @click="addOptions('confAnysList')"></i>
                            </template>
                            <template slot-scope="scope">
                                <i class="el-icon-edit"  @click="confAnysListUpdate(scope)"></i>
                                <i class="el-icon-delete"  @click="confAnysListDelete(scope)"></i>
                            </template>
                        </el-table-column>
                        </el-table>
                </el-form-item>
         </div>
         <!--解析设置end -->
         <!-- 任务计划 -->
         <h4 class="model-title">
             任务计划
         </h4>
         <div class="model-container">
                 <h4 style="height:20px;">运行该项目……</h4>
                  <el-form-item prop="portTaskPortId" class="extend-length"
                                >
                         <el-radio v-model="otherItem.portTask" label="0" style="margin-top:10px;">
                            当
                        </el-radio>
                        <el-select class="form-input" style="width:150px;"
                                    v-model="otherItem.portTaskPortId"
                                    clearable>
                                <el-option v-for="(text,value) in selectOptions.project" :key="value" :value="value" :label="text"></el-option>        
                            </el-select>
                            结束后运行
                </el-form-item>
                    <el-form-item prop="portTask"
                                >
                        <el-radio v-model="otherItem.portTask"  >
                            在系统启动后
                            <el-input-number style="width:150px;" v-model="otherItem.portTaskStartTime"  :min="0"  ></el-input-number>
                            分钟后运行
                        </el-radio>
                        <el-radio v-model="otherItem.portTask" label="3">每分钟</el-radio>
                        <el-radio v-model="otherItem.portTask" label="4">每小时</el-radio>
                        <el-radio v-model="otherItem.portTask" label="5">每天</el-radio>
                        <el-radio v-model="otherItem.portTask" label="6">每周</el-radio>
                        <el-radio v-model="otherItem.portTask" label="7">每月</el-radio>
                        <el-radio v-model="otherItem.portTask" label="8">每年</el-radio>
                        <el-radio v-model="otherItem.portTask" label="9">运行一次</el-radio>
                    </el-form-item>
                      <el-form-item prop="portTaskOrder" class="extend-length"
                                >
                          <el-radio v-model="otherItem.portTask" label="10">其他</el-radio>
                             <times style="width:255px;display: inline-block;" :value="otherItem.portTaskOrder" @changeValue="changeValue" type="1" ref="times"></times>
                            时间表达式
                     </el-form-item>
                     <el-form-item prop="portTaskLongTime"
                                >
                            运行时长
                            <el-input-number style="width:150px;" v-model="otherItem.portTaskLongTime"  :min="0"  ></el-input-number>
                            分钟（0=无限制）
                     </el-form-item>
         </div>
         <!--任务计划end -->
         </el-form>
         <!-- 项目状态 -->
         <h4 class="model-title">
             项目状态
         </h4>
         <div class="model-container">
             <p class="model-status-row">
                启动次数: {{portStatus.portRunNum}} 次 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上次启动时间：{{portStatus.portStateLastTime}}
             </p>
             <p class="model-status-row">
                待运行URL：<span class="model-status-result">{{portStatus.portStateRunNum}}个</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                成功运行URL：<span class="model-status-result">{{portStatus.portStateSusNum}}个</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                失败运行URL：<span class="model-status-result">{{portStatus.portStateErrNum}}个</span> 
             </p>
             <p class="model-status-row clearfix">
                <span class="f-left">任务状态：{{portStatus.portStateState==='0'?'运行中':'已停止'}}</span>
                <el-progress class="f-left" style="width:calc(100% - 170px);margin:10px;" :percentage="portStatus.portStateStatus"></el-progress>
             </p>
            <p class="model-status-row model-status-row-button">
               <el-button v-if="this.$route.query.protId && portStatus.portStateState==='0'">停止运行</el-button>
               <el-button v-if="this.$route.query.protId && portStatus.portStateState==='1'">立即运行</el-button>
               <el-button @click="saveAll" >保存</el-button>
               <el-button @click="resetAll">重置</el-button>
             </p>
         </div>
         <!--项目状态end -->
         <!-- 系统环境明细 -->
          <el-dialog title="选择系统环境"
               :visible.sync="visable.confSysList"
               width="45%"
               custom-class="dialog-default fixedHeight"
               >
            <div class="dialog-contant-default" v-if="visable.confSysList">
              <system-environment :operateId="operateId" :operateStatus="operateStatus" ref="systemEnvironment"></system-environment>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confSysList = false">取消</el-button>
              <el-button type="primary"
                        @click="saveSystemEnvironment()">保存</el-button>
            </div>
          </el-dialog>
         <!-- 代理服务明细 -->
          <el-dialog title="选择代理服务"
               :visible.sync="visable.confAgntList"
               width="45%"
               custom-class="dialog-default fixedHeight"
               >
            <div class="dialog-contant-default" v-if="visable.confAgntList">
              <agent-server :operateId="operateId" :operateStatus="operateStatus" ref="agentServer"></agent-server>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confAgntList = false">取消</el-button>
              <el-button type="primary"
                        @click="saveAgentServer()">保存</el-button>
            </div>
          </el-dialog>
         <!-- 不跟踪HTML -->
          <el-dialog title="不跟踪HTML"
               :visible.sync="visable.confFiltList"
               width="45%"
               custom-class="dialog-default fixedHeight"
               >
            <div class="dialog-contant-default" v-if="visable.confFiltList">
              <conf-fit :operateId="operateId" :operateStatus="operateStatus" ref="confFiltList"></conf-fit>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confFiltList = false">取消</el-button>
              <el-button type="primary"
                        @click="saveConfFiltList()">保存</el-button>
            </div>
          </el-dialog>
         <!-- 下载文件明细 -->
          <el-dialog :title="operateStatus === 1?'下载类型和大小的文件新增':'下载类型和大小的文件修改'"
               :visible.sync="visable.confDonList"
               width="40%"
               custom-class="dialog-default"
               >
            <div class="dialog-contant-default" v-if="visable.confDonList">
              <conf-don  :operateId="operateId" :operateStatus="operateStatus" ref="confDonList"></conf-don>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confDonList = false">取消</el-button>
              <el-button type="primary"
                        @click="saveConfDonList()">保存</el-button>
            </div>
          </el-dialog>
         <!-- 解析规则明细 -->
          <el-dialog :title="operateStatus === 1?'解析规则新增':'解析规则修改'"
               :visible.sync="visable.confAnysList"
               width="40%"
               custom-class="dialog-default"
               >
            <div class="dialog-contant-default" v-if="visable.confAnysList">
              <conf-anys  :operateId="operateId" :operateStatus="operateStatus" ref="confAnysList"></conf-anys>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confAnysList = false">取消</el-button>
              <el-button type="primary"
                        @click="saveConfAnysList()">保存</el-button>
            </div>
          </el-dialog>
         <!-- 创建项目库 -->
          <el-dialog title="创建项目库"
               :visible.sync="visable.confSource"
               width="40%"
               custom-class="dialog-default"
               >
            <div class="dialog-contant-default" v-if="visable.confSource">
              <conf-source  :operateId="operateId" :operateStatus="operateStatus" ref="confSource"></conf-source>
            </div>
                  <div slot="footer"
                class="dialog-footer">
              <el-button @click="visable.confSource = false">取消</el-button>
              <el-button type="primary"
                        @click="saveConfSource()">保存</el-button>
            </div>
          </el-dialog>
     </div>
 </template>
 <script>
 import { baseRequest } from '@/api/base'
 import { deepClone } from '@/utils'
 import times from '@/components/cronTrigger/triggerIndex'
 import SystemEnvironment from './optionComponents/SystemEnvironment'
 import ConfDon from './optionComponents/ConfDon'
 import AgentServer from './optionComponents/AgentServer'
 import ConfFit from './optionComponents/ConfFit'
 import ConfAnys from './optionComponents/ConfAnys'
 import ConfSource from './optionComponents/ConfSource'
 export default {
   name: 'taskOption',
   components: {
     times,
     SystemEnvironment,
     AgentServer,
     ConfDon,
     ConfFit,
     ConfAnys,
     ConfSource
   },
   mounted() {
     this.getProjectSelect()
     this.getSourceSelect().then(_ => {
       if (!this.$route.query.protId) {
         this.baseItem = deepClone(this.oriItem)
         this.otherItem = deepClone(this.tempItem)
         this.$nextTick(_ => {
           this.$refs.baseForm.clearValidate()
         })
       } else {
         baseRequest('/confProtInfo/select', { protId: this.$route.query.protId }).then(response => {
           const item = response.data.item
           for (const key in item) {
             if (this.oriItem.hasOwnProperty(key)) {
               this.oriItem[key] = item[key]
             }
             if (this.tempItem.hasOwnProperty(key)) {
               this.tempItem[key] = item[key]
             }
           }
           this.baseItem = deepClone(this.oriItem)
           this.otherItem = deepClone(this.tempItem)
           if (this.otherItem.portAnysSource || this.otherItem.portAnysSource === 0) {
             this.getTableSelect(this.otherItem.portAnysSource)
           }
         })
       }
     })
   },
   computed: {
     portTask: function() {
       return this.otherItem.portTask
     }
   },
   watch: {
     portTask(val) {
       this.validateFieldReload()
     }
   },
   data() {
     return {
       operateStatus: 1, // 1添加 2修改
       operateId: null,
       oriItem: {
         protName: null,
         protNameText: null,
         protAuthor: null,
         protStart: '1',
         protWebUri: null,
         protWebName: null,
         protWebNameText: null,
         confSysList: [],
         confAgntList: [],
         protState: '1'
       },
       baseItem: {},
       baseRules: {
         protName: [
           { required: true, message: '请填写此项' },
           { max: 256, message: '不能超过256个字' }
         ],
         protNameText: [
           { max: 1024, message: '不能超过1024个字' }
         ],
         protAuthor: [
           { required: true, message: '请填写此项' },
           { max: 36, message: '不能超过36个字' }
         ],
         protWebUri: [
           { required: true, message: '请填写此项' },
           { max: 1024, message: '不能超过1024个字' },
           { validator: (rule, value, callback) => {
             if (/(http|https|ftp):\/\/([\w.]+\/?)\S*/.test(value)) {
               callback()
             } else {
               callback(new Error('格式不正确'))
             }
           } }
         ],
         protWebName: [
           { required: true, message: '请填写此项' },
           { max: 256, message: '不能超过256个字' }
         ],
         protWebNameText: [
           { max: 1024, message: '不能超过1024个字' }
         ]
       },
       otherItem: {},
       ortherRules: {
         // 线程控制
         protMaxT: [
           { required: true, message: '请填写此项' },
           { validator: (rule, value, callback) => {
             if (value >= this.otherItem.protMinT) {
               callback()
             } else {
               callback(new Error('不能小于最小线程数'))
             }
           } }
         ],
         protMinT: [
           { required: true, message: '请填写此项' },
           { validator: (rule, value, callback) => {
             if (value <= this.otherItem.protMaxT) {
               callback()
             } else {
               callback(new Error('不能大于最大线程数'))
             }
           } }
         ],
         // 下载选项
         protDolPath: [
           { required: true, message: '请填写此项' },
           { max: 256, message: '不能超过256个字' }
         ],
         protDolLength: [
           { required: true, message: '请填写大小限制' }
         ],
         // 排除选项
         protFiltChar: [
           { max: 4000, message: '不能超过4000个字' }
         ],
         protDolLengthStart: [
           { max: 4000, message: '不能超过4000个字' }
         ],
         // 网络礼节
         protCoryTime: [
           { required: true, message: '请填写此项' }
         ],
         protCoryUseField: [
           { max: 1024, message: '不能超过1024个字' },
           { validator: (rule, value, callback) => {
             if (!value || /(http|https):\/\/([\w.]+\/?)\S*/.test(value)) {
               callback()
             } else {
               callback(new Error('输入格式不正确'))
             }
           } }
         ],
         protCoryUser: [
           { max: 1024, message: '不能超过1024个字' },
           { validator: (rule, value, callback) => {
             if (this.otherItem.protCoryType === '4') {
               if (value) {
                 if (/(http|https):\/\/([\w.]+\/?)\S*/.test(value)) {
                   callback()
                 } else {
                   callback(new Error('输入格式不正确'))
                 }
               } else {
                 callback(new Error('请填写身份'))
               }
             } else {
               if (!value || /(http|https):\/\/([\w.]+\/?)\S*/.test(value)) {
                 callback()
               } else {
                 callback(new Error('输入格式不正确'))
               }
             }
           } }
         ],
         // 探测设置
         protSearchPicNum: [
           { required: true, message: '请填写此项' }
         ],
         protSearchNum: [
           { required: true, message: '请填写此项' }
         ],
         protSearchFileNum: [
           { required: true, message: '请填写此项' }
         ],
         // 队列设置
         portQueueRun: [
           { required: true, message: '请填写此项' }
         ],
         portQueueRunTime: [
           { required: true, message: '请填写此项' }
         ],
         portQueueErr: [
           { required: true, message: '请填写此项' }
         ],
         portQueueErrTime: [
           { required: true, message: '请填写此项' }
         ],
         portQueueSus: [
           { required: true, message: '请填写此项' }
         ],
         portQueueSusTime: [
           { required: true, message: '请填写此项' }
         ],
         // 解析设置
         portAnysAllText: [
           { max: 1024, message: '不能超过1024个字' }
         ],
         // 任务计划
         portTaskPortId: [
           { validator: (rule, value, callback) => {
             if (value === this.$route.query.protId) {
               callback(new Error('不能选择项目自身'))
             } else {
               callback()
             }
           }
           },
           { validator: (rule, value, callback) => {
             if (this.otherItem.portTask === '0') {
               if (value) {
                 callback()
               } else {
                 callback(new Error('请选择项目'))
               }
             } else {
               callback()
             }
           } }
         ],
         portTaskOrder: [
           { validator: (rule, value, callback) => {
             if (this.otherItem.portTask === '10') {
               if (value) {
                 callback()
               } else {
                 callback(new Error('请输入时间表达式'))
               }
             } else {
               callback()
             }
           } }
         ],
         portTask: [
           { validator: (rule, value, callback) => {
             if (this.otherItem.portTaskStartTime || this.otherItem.portTaskStartTime === 0) {
               callback()
             } else {
               callback(new Error('请输入启动后时间'))
             }
           } }
         ],
         portTaskLongTime: [
           { required: true, message: '请填写此项' }
         ]
       },
       tempItem: {
         // 线程控制
         protMaxT: 10,
         protMinT: 1,
         protDelT: 100,
         // 下载选项
         protDolPath: '/gov.com/',
         protDolPathName: '0',
         protDolFileName: '1',
         protDolSaveHtml: '1',
         protDolUseMsie: '1',
         protDolChar: '1',
         protDolAdd: '1',
         protDolLocal: '0',
         protDolLengthStart: '0',
         protDolLength: '1024',
         confDonList: [],
         protDolFile: '1',
         protDolBackImg: '1',
         protDolJava: '1',
         protDolGroup: '1',
         // 排除选项
         protFiltCharUl: '0',
         protFiltAllChar: '1',
         protFiltChar: null,
         protFiltFileName: null,
         confFiltList: [],
         // 网络礼节
         protCoryField: '1',
         protCorySerd: '1',
         protCoryRemoves: '1',
         protCoryTime: 1,
         protCorySystem: '0',
         protCoryUseField: null,
         protCoryType: '3',
         protCoryUser: null,
         // 探测设置
         protSearchPic: '1',
         protSearchPicNum: 30,
         protSearchFile: '1',
         protSearchJs: '1',
         protSearchTable: '1',
         protSearchCookies: '1',
         protSearchNum: 5,
         protSearchFileNum: 5,
         // 更新设置
         portUpd: '0',
         portUpdHtml: '1',
         portUpdImg: '1',
         portUpdFile: '1',
         portUpdAll: '0',
         // 队列设置
         portQueueRun: 1000,
         portQueueRunTime: 600,
         portQueueErr: 1000,
         portQueueErrTime: 600,
         portQueueSus: 1000,
         portQueueSusTime: 600,
         // 解析设置
         portAnysHtml: '1',
         portAnysJson: '1',
         portAnysAll: '1',
         portAnysAllText: null,
         portAnysSource: null,
         portAnysTable: null,
         portAnysTittle: '1',
         portAnysOrgText: '1',
         portAnysIssue: '1',
         portAnysEffective: '1',
         portAnysAnnul: '1',
         portAnysAbtName: '1',
         portAnysWeb: '1',
         portAnysOrder: '1',
         confAnysList: [],
         // 任务计划
         portTask: '5',
         portTaskPortId: null,
         portTaskStartTime: 10,
         portTaskOrder: null,
         portTaskLongTime: 60
       },
       portStatus: {
         portRunNum: 0,
         portStateLastTime: null,
         portStateRunNum: 0,
         portStateSusNum: 0,
         portStateErrNum: 0,
         portStateState: '1', // 0运行中 1已停止
         portStateStatus: 0
       },
       visable: {
         confSysList: false,
         confAgntList: false,
         confDonList: false,
         confFiltList: false,
         confAnysList: false,
         confSource: false
       },
       selectOptions: {
         source: {},
         table: {},
         project: {}
       }
     }
   },
   methods: {
     // 获取目标库
     getSourceSelect() {
       return new Promise(resolve => {
         baseRequest('/confSource/getConfSourceSelect').then(response => {
           this.selectOptions.source = response.data.item
           resolve()
         })
       })
     },
     //  获取目标表
     getTableSelect(val) {
       baseRequest('/confSource/getTableNameSelect', { sourceId: val }).then(response => {
         this.selectOptions.table = response.data.item
       })
     },
     // 获取项目下拉
     getProjectSelect() {
       baseRequest('/confProtInfo/getProtInfoSelect').then(response => {
         this.selectOptions.project = response.data.item
       })
     },
     addOptions(key) {
       this.operateStatus = 1
       this.operateId = null
       this.visable[key] = true
     },
     updateOptions(key, name, id) {
       this.operateStatus = 2
       this.operateId = id
       this.visable[key] = true
     },
     deleteOptions(key, name, id) {
       this.baseItem[key] = this.baseItem[key].filter(item => item[name] !== id)
     },
     deleteOtherOptions(key, name, id) {
       this.otherItem[key] = this.otherItem[key].filter(item => item[name] !== id)
     },
     saveSystemEnvironment() {
       this.$refs.systemEnvironment.saveCurrent(item => {
         if (this.operateStatus === 1) {
           if (this.baseItem.confSysList.find(i => i.sysId === item.sysId)) {
             this.$message.warning('该数据已存在')
           } else {
             this.baseItem.confSysList.push(item)
             this.visable.confSysList = false
           }
         } else {
           if (this.baseItem.confSysList.find(i => i.sysId !== this.operateId && i.sysId === item.sysId)) {
             this.$message.warning('该数据已存在')
           } else {
             for (const iterator of this.baseItem.confSysList) {
               if (iterator.sysId === this.operateId) {
                 iterator.sysId = item.sysId
                 iterator.sysName = item.sysName
                 this.visable.confSysList = false
               }
             }
           }
         }
       })
     },
     saveAgentServer() {
       this.$refs.agentServer.saveCurrent(item => {
         if (this.operateStatus === 1) {
           if (this.baseItem.confAgntList.find(i => i.agntId === item.agntId)) {
             this.$message.warning('该数据已存在')
           } else {
             this.baseItem.confAgntList.push(item)
             this.visable.confAgntList = false
           }
         } else {
           if (this.baseItem.confAgntList.find(i => i.agntId !== this.operateId && i.agntId === item.agntId)) {
             this.$message.warning('该数据已存在')
           } else {
             for (const iterator of this.baseItem.confAgntList) {
               if (iterator.agntId === this.operateId) {
                 iterator.agntId = item.agntId
                 iterator.agntName = item.agntName
                 this.visable.confAgntList = false
               }
             }
           }
         }
       })
     },
     saveConfFiltList() {
       this.$refs.confFiltList.saveCurrent(item => {
         if (this.operateStatus === 1) {
           if (this.otherItem.confFiltList.find(i => i.protFiltId === item.protFiltId)) {
             this.$message.warning('该数据已存在')
           } else {
             this.otherItem.confFiltList.push(item)
             this.visable.confFiltList = false
           }
         } else {
           if (this.otherItem.confFiltList.find(i => i.protFiltId !== this.operateId && i.protFiltId === item.protFiltId)) {
             this.$message.warning('该数据已存在')
           } else {
             for (const iterator of this.otherItem.confFiltList) {
               if (iterator.protFiltId === this.operateId) {
                 iterator.protFiltId = item.protFiltId
                 iterator.protFilt = item.protFilt
                 this.visable.confFiltList = false
               }
             }
           }
         }
       })
     },
     confDonListUpdate(scope) {
       this.operateStatus = 2
       this.operateId = scope
       this.visable.confDonList = true
     },
     confAnysListUpdate(scope) {
       this.operateStatus = 2
       this.operateId = scope
       this.visable.confAnysList = true
     },
     confDonListDelete(scope) {
       this.otherItem.confDonList.splice(scope.$index, 1)
     },
     confAnysListDelete(scope) {
       this.otherItem.confAnysList.splice(scope.$index, 1)
     },
     saveConfDonList() {
       this.$refs.confDonList.saveCurrent(item => {
         if (this.operateStatus === 1) {
           this.otherItem.confDonList.push(item)
           this.visable.confDonList = false
         } else {
           this.otherItem.confDonList.splice(this.operateId.$index, 1, item)
           this.visable.confDonList = false
         }
       })
     },
     saveConfAnysList() {
       this.$refs.confAnysList.saveCurrent(item => {
         if (this.operateStatus === 1) {
           this.otherItem.confAnysList.push(item)
           this.visable.confAnysList = false
         } else {
           this.otherItem.confAnysList.splice(this.operateId.$index, 1, item)
           this.visable.confAnysList = false
         }
       })
     },
     saveConfSource() {
       this.$refs.confSource.saveCurrent(item => {
         this.getSourceSelect().then(_ => {
           this.$message.success('添加成功')
           this.visable.confSource = false
         })
       })
     },
     validateFieldReload() {
       this.$refs.otherForm.validateField('portTaskPortId')
       this.$refs.otherForm.validateField('portTaskOrder')
     },
     changeValue(val) {
       this.otherItem.portTaskOrder = val
       this.$refs.otherForm.validateField('portTaskOrder')
     },
     reset() {
       this.baseItem = deepClone(this.oriItem)
     },
     save() {
       this.$refs.baseForm.validate((valid) => {
         if (valid) {
           const data = Object.assign(this.baseItem, this.tempItem)
           if (!this.$route.query.protId) {
             data.portRunNum = '0'
             data.portStateRunNum = '0'
             data.portStateSusNum = '0'
             data.portStateErrNum = '0'
             data.portStateState = '1'
             data.portStateStatus = 0
             baseRequest('/confProtInfo/add', data).then(_ => {
               this.$message.success('操作成功')
               this.$router.push({ name: 'task' })
             })
           } else {
             data.protId = this.$route.query.protId
             baseRequest('/confProtInfo/update', data).then(_ => {
               this.$message.success('操作成功')
               this.$router.push({ name: 'task' })
             })
           }
         }
       })
     },
     resetAll() {
       this.baseItem = deepClone(this.oriItem)
       this.otherItem = deepClone(this.tempItem)
     },
     saveAll() {
       let validateResult = true
       this.$refs.baseForm.validate((valid) => {
         if (!valid) {
           validateResult = false
         }
       })
       this.$refs.otherForm.validate((valid) => {
         if (!valid) {
           validateResult = false
         }
       })
       if (validateResult) {
         const data = Object.assign(this.baseItem, this.otherItem)
         if (!this.$route.query.protId) {
           data.portRunNum = '0'
           data.portStateRunNum = '0'
           data.portStateSusNum = '0'
           data.portStateErrNum = '0'
           data.portStateState = '1'
           data.portStateStatus = 0
           baseRequest('/confProtInfo/add', data).then(_ => {
             this.$message.success('操作成功')
             this.$router.push({ name: 'task' })
           })
         } else {
           data.protId = this.$route.query.protId
           baseRequest('/confProtInfo/update', data).then(_ => {
             this.$message.success('操作成功')
             this.$router.push({ name: 'task' })
           })
         }
       }
     }
   }
 }
 </script>
 <style lang="scss" scoped>
 i.el-icon-edit, i.el-icon-delete,i.el-icon-plus{
     cursor: pointer;
     &:hover{
         color:#409EFF;
     }
 }
 .f-left{
     float:left;
 }
 .f-right{
     float:right;
 }
 </style>
 
 <style lang="scss" >
 .task-option{
     color: #606266;
    font-weight: 500;
    font-size: 14px;
     padding: 0px 80px 50px;
     .model-title{
         height: 40px;
         line-height: 40px;
         margin-top:10px;
     }
     .model-container{
         padding:20px 30px;
         border:1px solid #909399;
         border-radius: 7px;
         .extend-length{
            width: 100%;
            .el-form-item__content{
                width: calc(100% - 100px);
            }
            .form-input{
                width: 100%;
            }
            &.long-length{
                .el-form-item__content{
                    width: calc(100% - 160px);
                }
            }
            &.whole-length{
                .el-form-item__content{
                    width: 100%;
                }
            }
         }
         .extend-length-href{
             width:calc(50% - 20px);
             .el-form-item__content{
                 width: calc(100% - 85px)
             }
         }
         .result-list{
             list-style: none;
             float: left;
             width: calc(100% - 80px);
             border: 1px solid #409EFF;
             padding:0px 10px;
             li{
                position: relative;
                overflow:hidden;
                text-overflow:ellipsis;
                white-space:nowrap;
                .result-option{
                    position: absolute;
                    top:0;
                    right: 0;
                    i{
                        cursor: pointer;
                        &:hover{
                            color:#409EFF;
                        }
                    }
                }
             }
         }
         .add-result{
             float: left;
             margin-left:5px;
         }
         .model-status-row{
             line-height:40px;
             .model-status-result{
                 color:#409EFF;
             }
             &.model-status-row-button{
                 text-align:center;
             }
         }
     }
 }
 </style>
 
