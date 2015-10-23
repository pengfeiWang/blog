    var get=function(id){return document.getElementById(id)};
    String.prototype.trim=function(){return this.replace(/^\s+/,'').replace(/\s+$/,'')};
    function unicode(){
        var preStr='\\u';var value=get('ch2uncd-content').value.trim();
        var cnReg=/[\u0391-\uFFE5]/gm;
        if(cnReg.test(value)){
            var ret=value.replace(cnReg,function(str){
                return preStr+str.charCodeAt(0).toString(16)
            });
            get('ch2uncd-content').value=ret
        }else{
            alert('没有找到中文字符串')
        }
    }
    function chinese(){
        var ovalue=get('ch2uncd-content').value.trim();
        if(ovalue){
            var omt=ovalue,uReg=/\\u(\w{4})/img;
            if(uReg.test(ovalue)){
                var ret=ovalue.replace(uReg,function(str,subs){
                    return unescape('%u'+subs)
                });
                get('ch2uncd-content').value=ret
            }
        }else{
            alert('没有输入内容')
        }
    }