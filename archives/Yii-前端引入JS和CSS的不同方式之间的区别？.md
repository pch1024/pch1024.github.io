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

- 加载优先级：控制层 > main > views
    ```
    public function init() 
    {    
        //parent::init();    
        Yii::app()->clientScript->registerCssFile(Yii::app()->baseUrl.'/css/my.css'); 
        Yii::app()->clientScript->registerScriptFile(Yii::app()->baseUrl.'/css/my.js'); 
    } 
    ```
