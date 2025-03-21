```javascript
// 发送post请求
function httpPost(url, params, target)
{
    var temp = document.createElement("form");
    temp.action = url;
    temp.method = "post";
    temp.style.display = "none";
    if (target != null && target != '') {
        temp.target = target;
    }
 
    for (var x in params) {
        var opt = document.createElement('textarea');
        opt.name = x;
        opt.value = params[x];
        temp.appendChild(opt);
    }
 
    document.body.appendChild(temp);
    temp.submit();
    document.body.removeChild(temp);
}
 
// 导出报表
function exportSheet(sheetid, filename) {
    var html = '<html><head><meta charset="utf-8" /></head><body style="text-align: center">' + document.getElementById(sheetid).outerHTML + '</body></html>';
    var blob = new Blob([html], {type: "application/vnd.ms-excel"});
    var a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = filename;
    if((window.navigator.userAgent).indexOf('Firefox') >= 0){
        // 优化火狐浏览器无法下载情况
        a.dispatchEvent(new MouseEvent('click'));
    }else{
        a.click();
    }
    a = null;
}
 
// 第二种导出报表的方法，似乎没上面这种简便好用，先用等一种
/*****************************************************/
function exportSheet2(tableid) {
    if ('ie' === getExplorer()) {
        var curTbl = document.getElementById(tableid);
        var oXL = new ActiveXObject('Excel.Application');
        var oWB = oXL.Workbooks.Add();
        var xlsheet = oWB.Worksheets(1);
        var sel = document.body.createTextRange();
        sel.moveToElementText(curTbl);
        sel.select();
        sel.execCommand('Copy');
        xlsheet.Paste();
        oXL.Visible = true;
 
        try {
            var fname = oXL.Application.GetSaveAsFilename('Excel.xls', 'Excel Spreadsheets (*.xls), *.xls');
        } catch (e) {
            print("Nested catch caught " + e);
        } finally {
            oWB.SaveAs(fname);
            oWB.Close(savechanges = false);
            oXL.Quit();
            oXL = null;
            idTmr = window.setInterval('Cleanup();', 1);
        }
    } else {
        tableToExcel(tableid);
    }
}
 
function getExplorer()
{
    var explorer = window.navigator.userAgent;
    if (explorer.indexOf("MSIE") >= 0) {
        return 'ie';
    } else if (explorer.indexOf("Firefox") >= 0) {
        return 'Firefox';
    } else if (explorer.indexOf("Chrome") >= 0) {
        return 'Chrome';
    } else if (explorer.indexOf("Opera") >= 0) {
        return 'Opera';
    } else if (explorer.indexOf("Safari") >= 0) {
        return 'Safari';
    } else {
        return 'unknown type';
    }
}
 
function Cleanup() {
    window.clearInterval(idTmr);
    CollectGarbage();
}
 
var tableToExcel = (function() {
    var uri = 'data:application/vnd.ms-excel;base64,',
        template = '<html><head><meta charset="utf-8"></head><body><table border="1">{table}</table></body></html>',
        base64 = function (s) {
            return window.btoa(unescape(encodeURIComponent(s)));
        },
        format = function(s, c) {
            return s.replace(/{(\w+)}/g, function(m, p) {
                return c[p];
            });
        };
    return function(table, name) {
        if (!table.nodeType) {
            table = document.getElementById(table);
        }
        var ctx = {
            worksheet: name || 'Worksheet',
            table: table.innerHTML
        };
        window.location.href = uri + base64(format(template, ctx));
    }
})();
/*****************************************************/
 
function getJsonData(days) {
    var newDate = new Date();
    var lastDate = getDay(newDate,days);
    getJsonList(newDate,lastDate)
 
}
 
function getDay(beginDate, days) {
    var beginDate = beginDate.split("-");
    var nDate = new Date(beginDate[1] + '-' + beginDate[2] + '-' + beginDate[0]); //转换为MM-DD-YYYY格式
    var millSeconds = Math.abs(nDate) + (days * 24 * 60 * 60 * 1000);
    var rDate = new Date(millSeconds);
    var year = rDate.getFullYear();
    var month = rDate.getMonth() + 1;
    if (month < 10) month = "0" + month;
    var date = rDate.getDate();
    if (date < 10) date = "0" + date;
    return (year + "-" + month + "-" + date);
}
 
 
function getCurrentTime() {
    var date = new Date();
    var seperator1 = '-';
    var seperator2 = ':';
    date.setDate(date.getDate());
    var month = PrefixInteger(date.getMonth() + 1);
    var strDate = PrefixInteger(date.getDate());
    var hours = PrefixInteger(date.getHours());
    var minutes = PrefixInteger(date.getMinutes());
    var seconds = PrefixInteger(date.getSeconds());
    var date = date.getFullYear() + seperator1 + month + seperator1 + strDate;
    var time = hours + seperator2 + minutes + seperator2 + seconds;
    $('.date').html(date);
    $('.time').html(time)
}
 
function PrefixInteger(num) {
    return (Array(2).join('0') + num).slice(-2)
}
 
function backUrl(pageUrl) {
    window.location.href=pageUrl;
}
 
// $('#startDate').datetimepicker({
//     formatTime:'H:i',
//     formatDate:'d.m.Y',
//     //defaultDate:'8.12.1986', // it's my birthday
//     defaultDate:'+03.01.1970', // it's my birthday
//     defaultTime:'10:00',
//     timepickerScrollbar:false
// });
// $('#lastDate').datetimepicker({
//     formatTime:'H:i',
//     formatDate:'d.m.Y',
//     //defaultDate:'8.12.1986', // it's my birthday
//     defaultDate:'+03.01.1970', // it's my birthday
//     defaultTime:'10:00',
//     timepickerScrollbar:false
// });
 
 
//获取时间列表
function getTimeList()
{
    //60 天
    var str1 = '';
    var str2 = '';
    var days = [];
    var date = new Date();
    var name = [' (今天) ',' (昨天) ',' (前天) '];
    for (var i = 0; i < 60; i++) {
        var year = date.getFullYear();
        var month = date.getMonth() + 1;
        var day = date.getDate();
        if(month < 10){
            month = '0' + month;
        }
        if(day < 10){
            day = '0' + day;
        }
 
        if(i < 3){
            str1 = year + '-' + month + '-' + day + name[i];
        }else{
            str1 = year + '-' + month + '-' + day;
        }
        str2 =  year + '-' + month + '-' + day;
 
        days.push([str1,str2]);
        date.setTime(date.getTime() - 86400000);
    }
 
    //25 周
    var weeks = [];
    var date = new Date();
    var value = [6, 0, 1, 2, 3, 4, 5];
    for (var i = 1; i <= 25; i++) {
        var index = date.getDay();
        date.setTime(date.getTime() - (7 + value[index]) * 86400000);
        var year = date.getFullYear();
        var month = date.getMonth() + 1;
        var day = date.getDate();
        if(month < 10){
            month = '0' + month;
        }
        if(day < 10){
            day = '0' + day;
        }
 
        if(i < 2){
            str1 = year + '-' + month + '-' + day + ' (上周)';
        }else{
            str1 = year + '-' + month + '-' + day;
        }
        str2 = year + '-' + month + '-' + day;
 
        weeks.push([str1,str2]);
    }
 
    //12 月
    var months = [];
    var date = new Date();
    var cur_year = date.getFullYear();
    var cur_month = date.getMonth();
    var prev_year = cur_year;
    var prev_month = cur_month + 1;
    for (var i = 1; i <= 12; i++) {
        if(prev_month >= 2){
            prev_month = prev_month - 1;
        }else{
            prev_year = prev_year - 1;
            prev_month = 12;
        }
 
 
        if(prev_month < 10){
            prev_month = '0' + prev_month;
        }
 
        if(i < 2){
            str1 = prev_year + '-' + prev_month + '-01' + ' (上月)';
        }else{
            str1 = prev_year + '-' + prev_month + '-01';
        }
        str2 = prev_year + '-' + prev_month + '-01';
 
        months.push([str1,str2]);
    }
 
    return {
        'day': days,
        'week': weeks,
        'month': months
    };
}
 
//显示时间列表数据
function showTimeList(timeUnit, timeArea) {
    var str = '';
    var timeList = getTimeList();
    if(typeof(timeUnit) != 'undefined' && timeUnit != null && timeUnit.length > 0 && typeof(timeList[timeUnit]) != 'undefined'){
        for(var i = 0;i<timeList[timeUnit].length;i++){
            if(timeArea == timeList[timeUnit][i][1]){
                str += '<option value="'+timeList[timeUnit][i][1]+'" selected="selected">'+timeList[timeUnit][i][0]+'</option>';
            }else{
                str += '<option value="'+timeList[timeUnit][i][1]+'">'+timeList[timeUnit][i][0]+'</option>';
            }
        }
    }
 
    return str;
}
 
//验证日期格式
function checkDateFmt(val) {
    var reg = /^(\d{4}-\d{2}-\d{2})$/;
    if (typeof(val) == 'undefined' || val == null || val.length <= 0 || !reg.test(val)) {
        alert('日期格式不正确');
        return;
    }
}
 
//验证时间格式
function checkTimeUnitFmt(val) {
    var timeUnit = ['day','week','month'];
    if (typeof(val) == 'undefined' || val == null || val.length <= 0 || $.inArray(val,timeUnit) < 0) {
        alert('时间格式不正确');
        return;
    }
}
```
