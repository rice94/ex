# ex
var mypass = "5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8"; //ПАРОЛЬ

function kill_ctrl(e) 
{
    var forbiddenKeys = new Array('a', 'c', 'x', 's');
    var key;
    var isCtrl;
    
    key = e.which; // ОБРАБОТКА КЛАВИШ ДЛЯ ДРУГИХ
    if (e.ctrlKey) isCtrl = true;
    else isCtrl = false;
    
    
    if (isCtrl) //ЕСЛИ ЗАЖАТ CTRL ОБРАБАТЫВАЕМ НАЖАТИЕ КЛАВИШ ИЗ МАССИВА
    {
        for (i = 0; i < forbiddenKeys.length; i++) 
        { 
            if (forbiddenKeys[i].toLowerCase() == String.fromCharCode(key).toLowerCase()) 
            {
                return false;
            }
        }
    }
   
    if (e.which == 81)  //ПРОВЕРКА ПРАВИЛЬНОСТИ ПАРОЛЯ И СНЯТИЕ ЗАЩИТЫ ПРИ НАЖАТИИ НА "Q"
    {
       var pass = prompt("Введите пароль","");
       if (CryptoJS.SHA1(pass) == mypass) 
        {
            alert("Запрет снят");
            security_off();
        }
    }
    return true;
}

function disable_option(target) // ЗАПРЕТ НА ВЫДЕЛЕНИЕ
{
    if (typeof target.style.MozUserSelect != "undefined") 
    {
        target.style.MozUserSelect = "none"; //Firefox
    }
    if (typeof target.style.WebkitUserSelect != "undefined") 
    {
        target.style.WebkitUserSelect = "none"; //WebKit
    }
    target.style.cursor = "default";
}

function enable_option(target) 
{
    if (typeof target.style.MozUserSelect != "undefined") 
    {
        target.style.MozUserSelect = "auto";
    }
    if (typeof target.style.WebkitUserSelect != "undefined") 
    {
        target.style.WebkitUserSelect = "auto";
    }
}
function click_NS(e) 
{ 
    if (document.layers || (document.getElementById && !document.all)) 
    {
        if (e.which == 2 || e.which == 3) 
        {
            return false;
        }
    }
    return true;
}

function security_on() 
{
    disable_option(document.body); //ССЫЛАЕТСЯ НА ТЕГ <BODY>
    
    //ПЕРЕХВАТ НАЖАТИЯ КНОПКИ МЫШИ, НАСТУПИВШИЕ В ДРУГИХ ЭЛЕМЕНТАХ СТРАНИЦЫ
    document.captureEvents(Event.MOUSEDOWN);
    
    //ОБРАБОТКА ПРАВОЙ КЛАВИШИ МЫШИ И КОЛЕСИКА
    document.onmousedown = click_NS; 
    
    //ЗАПРЕТ НА ВЫДЕЛЕНИЕ ЭЛЕМЕНТОВ СТРАНИЦЫ
    document.onselectstart = function() {return false};  
    
    //ЗАПРЕТ НА ВЫЗОВ КОНТЕКСТНОГО МЕНЮ
    document.oncontextmenu = function() {return false};  
    
    //ЗАПРЕТ НА ПЕРЕТАСКИВАНИЕ
    document.ondragstart = function() {return false}; 
    document.onkeydown = kill_ctrl;
}
 
function security_off() 
{
    enable_option(document.body); //ССЫЛАЕТСЯ НА ТЕГ <BODY>
    document.onkeydown = function() {return true};
    document.captureEvents(Event.MOUSEDOWN);
    document.onmousedown = function() {return true};
    document.onmouseup = function() {return true};
    document.onselectstart = function() {return true};
    document.ondragstart = function() {return true};
    document.oncontextmenu = function() {return true};
}

window.onload = function() //ВКЛЮЧЕНИЕ ОГРАНИЧЕНИЙ ПРИ ПЕРЕЗАГРУЗКЕ СТРАНИЦЫ 
{    
    security_on();
    alert("Запрет включен");
};



