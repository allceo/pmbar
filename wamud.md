```
// 常用脚本导入
// 自命令类型选 Raidjs流程
($repourl) = http://wsmud-cdn.if404.com/

@js WorkflowConfig.createFinder("杂务")
@js WorkflowConfig.createFinder("提升")

($f_ss)={"name":"0三三懒人包","source":"(repourl)/三三懒人包.flow.txt","finder":"根文件夹"}
($f_zdxy)={"name":"0自动襄阳","source":"(repourl)/襄阳/自动襄阳(扫墙版).flow.txt","finder":"杂务"}

($f_zdrc)={"name":"自动日常","source":"(repourl)/自动日常.flow.txt","finder":"根文件夹"}
($f_zcty)={"name":"自创推演","source":"(repourl)/自创推演.flow.txt","finder":"根文件夹"}
($f_yjxy)={"name":"一键咸鱼","source":"(repourl)/一键咸鱼.flow.txt","finder":"根文件夹"}
($f_zdzb)={"name":"自动追捕","source":"(repourl)/自动追捕.flow.txt","finder":"根文件夹"}
($f_zdwd)={"name":"自动武道","source":"(repourl)/自动武道.flow.txt","finder":"根文件夹"}
($f_kssd)={"name":"快速扫荡","source":"(repourl)/副本/快速扫荡.flow.txt","finder":"根文件夹"}
($f_tdfb)={"name":"偷渡副本","source":"(repourl)/副本/偷渡副本.flow.txt","finder":"根文件夹"}
($f_zdyb)={"name":"自动运镖","source":"(repourl)/运镖/自动运镖.flow.txt","finder":"根文件夹"}
($f_yjsg)={"name":"一键收割","source":"(repourl)/杂务/一键收割.flow.txt","finder":"杂务"}
($f_zdcbt)={"name":"自动藏宝图","source":"(repourl)/杂务/自动藏宝图.flow.txt","finder":"杂务"}
($f_zlmj)={"name":"整理秘籍","source":"(repourl)/杂务/整理秘籍.flow.txt","finder":"杂务"}
($f_lxdm)={"name":"练习代码生成器","source":"(repourl)/提升/练习代码生成器.flow.txt","finder":"提升"}
($f_yjxx)={"name":"一键学习","source":"(repourl)/提升/一键学习.flow.txt","finder":"提升"}
($f_jqlx)={"name":"精确练习","source":"(repourl)/提升/精确练习.flow.txt","finder":"提升"}
($f_mpjj)={"name":"门派进阶","source":"(repourl)/提升/门派进阶.flow.txt","finder":"提升"}
($f_yzdy)={"name":"研制绿丹","source":"(repourl)/杂务/研制丹药/研制绿丹.flow.txt","finder":"研制丹药"},{"name":"研制蓝丹","source":"(repourl)/杂务/研制丹药/研制蓝丹.flow.txt","finder":"研制丹药"},{"name":"研制黄丹","source":"(repourl)/杂务/研制丹药/研制黄丹.flow.txt","finder":"研制丹药"},{"name":"研制紫丹","source":"(repourl)/杂务/研制丹药/研制紫丹.flow.txt","finder":"研制丹药"},{"name":"研制橙丹","source":"(repourl)/杂务/研制丹药/研制橙丹.flow.txt","finder":"研制丹药"},{"name":"研制突破","source":"(repourl)/杂务/研制丹药/研制突破.flow.txt","finder":"研制丹药"}
($f_sjxl)={"name":"书架香炉","source":"(repourl)/杂务/书架香炉.flow.txt","finder":"根文件夹"}
($f_yywd)={"name":"圆月弯刀","source":"(repourl)/副本/圆月弯刀.flow.txt","finder":"根文件夹"}
($f_gdzm)={"name":"古代宗门","source":"(repourl)/副本/古代宗门.flow.txt","finder":"根文件夹"}
($f_wsd)={"name":"武神殿","source":"(repourl)/副本/武神殿.flow.txt","finder":"根文件夹"}
($f_gzm)={"name":"古宗门躺尸","source":"(repourl)/副本/古宗门躺尸版.flow.txt","finder":"根文件夹"}

($f_list)=(f_list),(f_ss)
($f_list)=(f_list),(f_zdxy)

($f_list)=(f_list),(f_zdrc)
($f_list)=(f_list),(f_zcty)
($f_list)=(f_list),(f_yjxy)
($f_list)=(f_list),(f_zdzb)
($f_list)=(f_list),(f_zdwd)
($f_list)=(f_list),(f_kssd)
($f_list)=(f_list),(f_tdfb)
($f_list)=(f_list),(f_zdyb)
($f_list)=(f_list),(f_yjsg)
($f_list)=(f_list),(f_zdcbt)
($f_list)=(f_list),(f_zlmj)
($f_list)=(f_list),(f_lxdm)
($f_list)=(f_list),(f_yjxx)
($f_list)=(f_list),(f_jqlx)
($f_list)=(f_list),(f_mpjj)
($f_list)=(f_list),(f_yzdy)
($f_list)=(f_list),(f_sjxl)
($f_list)=(f_list),(f_yywd)
($f_list)=(f_list),(f_gdzm)
($f_list)=(f_list),(f_wsd)
($f_list)=(f_list),(f_gzm)

// 开始导入
($num)=1
[if] (f_list) != null
  @js ($l) = [(f_list)].length
  [while] (num)<(l)
    @js var time = Date.parse( new Date());var f=[(f_list)];var n=f[(num)]["name"];var s=f[(num)]["source"];var fd=f[(num)]["finder"];WorkflowConfig.removeWorkflow({"name":n,"type":"flow","finder":fd});$.get(s,{stamp:time},function(data,status){WorkflowConfig.createWorkflow(n,data,fd);});messageAppend("导入流程："+fd+" ==> <hiy>"+n+"</hiy>")
    ($num)=(num)+1

@print 导入触发：<ord>红boss报告</ord>
@js unsafeWindow.TriggerCenter.remove("红boss报告")
@await 350
@js Server.importTrigger("(rboss)")

@print 导入触发：<hic>卸武自装</hic>
@js unsafeWindow.TriggerCenter.remove("卸武自装")
@await 350
@js Server.importTrigger("白三三·卸武自装·触发@024da48fa09fd95b11f0af9046d7d33c")

@print 导入触发：<hic>帮战橙满伤</hic>
@js unsafeWindow.TriggerCenter.remove("橙满伤")
@await 350
@js Server.importTrigger("白三三·橙满伤·触发@4287ef6ec9f3737032defbdfb1e664dc")
@js WG.SendCmd("tm 如果满伤脱战过程不想卸武器，请在触发代码中按说明修改参数。")

@print 导入触发：<hic>襄阳叫杀</hic>
@js unsafeWindow.TriggerCenter.remove("襄阳叫杀")
@await 350
@js Server.importTrigger("白三三·襄阳叫杀·触发@07da8a7bfb68b5da972f4e158dbf4198")

@print 导入触发：<hic>襄阳满军功</hic>
@js unsafeWindow.TriggerCenter.remove("襄阳满")
@await 350
@js Server.importTrigger("白三三·襄阳满·触发@56725c6da1156bcb745bdd48402a0aaf")
@print <hiy>导入结束！</hiy>
```
