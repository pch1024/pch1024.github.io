<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta name="viewport" content="width=device-width,height=device-height,inital-scale=1.0,maximum-scale=1.0,user-scalable=no;" />
<!-- 自定义网页信息 -->
<title>网站首页</title>
<link rel="shortcut icon"   href="  " />
<base target="_blank"       href="  " />  
<meta name="description" content="	" />
<meta name="keywords"    content="	" />
<meta name="Copyright"   content="	" />
<meta name="author"      content="	" />
<!-- 外链样式表和脚本 -->
<link rel="stylesheet" type="text/css" href = "	">
<script type="text/javascript" 		    src = "  "></script>
</head>
<body>

#在@app\Views\index.php视图层引

- 最直接的方式，一个页面写一次，先写先引用

    ```
    Yii::app()->clientScript->registerScriptFile(Yii::app()->baseUrl . "/js/jqueryui/jquery-ui.min.js", CClientScript::POS_END);
    
    ```
    
- 批注1：在视图层引用与在控制层引用的方式一样。但在视图层中引用加载的要晚一些。
- 批注2：引用路径是使用baseUrl，而不是basePath。
- 批注3：关于参数CClientScript::POS_END，作用是延时加载，提高页面渲染效率。

全部参数一览：
`CClientScript::POS_HEAD ||CClientScript::POS_BEGIN ||CClientScript::POS_END ||CClientScript::POS_LOAD ||CClientScript::POS_READY ||`
- 注：这些参数仅适用于加载js文件，不适用于加载css文件。


#在../layouts/main.php中引入


1. ##直接引入

    ```
    <!-- css -->  
    <link rel="stylesheet" type="text/css" href="<?php echo Yii::app()->request->baseUrl; ?>/css/print.css" media="print" />  
    <!-- 图片 -->  
    <link rel="stylesheet" type="text/css" href="<?php echo Yii::app()->request->baseUrl; ?>/js/autocomplete/indicator.gif" />  
    <!-- js -->  
    <script type="text/javascript" src="<?php echo Yii::app()->request->baseUrl; ?>/js/jquery.js"></script>  
    ```

2. ##yii方式引入

    ```
    <?php 
    <!-- （一）简单用法 --> 
    <!-- js --> 
        Yii::app()->clientScript->registerScriptFile(Yii::app()->baseUrl . "/js/jqueryui/jquery-ui.min.js", CClientScript::POS_END); 
        
    <!-- （二）复杂用法 --> 
    if($this->user->id) { 
        Yii::app()->clientScript->registerScriptFile(Yii::app()->createUrl('/account/info', array('format' => 'js')), CClientScript::POS_END); 
    } 
    if($this->user->id) { 
        Yii::app()->clientScript->registerScriptFile(Yii::app()->createUrl('site/baseJs')); 
    } 
    ```

    - 批注：在yii运行后，第一种在head中，第二种在body最后面，显然后者效率更高。但必须加载的js和css有必要写在head中。

#在控制层(../controllers/xxController.php)添加CSS文件或JavaScript文件

- 加载优先级：控制层 > main > views（后面的覆盖前面的）
    ```
    public function init() 
    {    
        //parent::init();    
        Yii::app()->clientScript->registerCssFile(Yii::app()->baseUrl.'/css/my.css'); 
        Yii::app()->clientScript->registerScriptFile(Yii::app()->baseUrl.'/css/my.js'); 
    } 
    ```
    
#  新增： 
- 在控制层，还可以在ActionIndex中引入，而且还可以引入别的module文件夹中的js/css文件。甚至是任意文件夹下的js/css文件 

    ```
    public function actionIndex(){  
            $modify,$reg = some_value;  
            $js = $this->renderFile($this->getInstallViewPath(). '/asset/install.js',array('reg_mp'=>$reg), true);  
            $js = $this->renderFile($this->getViewPath() . '/assets/install_params.js', array('modify' => $modify), true);  
            
            $cs = Yii::app()->clientScript;  
            $cs->registerScript('asset/install', $js, CClientScript::POS_END);  
            $cs->registerCssFile(Yii::app()->baseUrl . '/css/launch_feed.css');  
            $cs->registerScript('assets/install_params',$js,CClientScript::POS_END);  
            $cs->registerScriptFile(Yii::app()->baseUrl . '/resources/jquery.form.js');  
            
            $cs->registerCssFile(Yii::app()->baseUrl . '/css/install_params.css'); 
             
            $this->render('xxx');      
        }  
    
    public function getInstallViewPath() {  
            return $this->getModule()->getBasePath().'/../operations/views';  
    }    
    ```
    
    
    


	
</body>
</html>
<!--
made with ♥ by:
          _
    _    |_|    _    
   |_|  _____  |_|    
 _    /       \    _
|_|  /    ____ \  |_|
    /    /    \ \                  
   /    /      \ \
  |\___/        \/    
   \                   
    \           / 
     \         /     
      |       |            
      |       | 
       \_____/    
-->		
