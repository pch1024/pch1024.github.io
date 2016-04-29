<input 
    onkeyup="value=value.replace(/[^\u4E00-\u9FA5]/g,'')" 
    onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^\u4E00-\u9FA5]/g,''))"
>

<input type="text" maxlength="11" />

<input type="text" placeholder="Tip" />