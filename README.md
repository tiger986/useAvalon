## The basic use of Avalon
#### HTML code
```html
<!DOCTYPE html>
<html>
<head>
    <title>上传</title>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="create.css">
	<script type="text/javascript" src="jquery.min.js"></script>
	<script type="text/javascript" src="main.js"></script>
<body>
<div class="body">
    <div style="margin-bottom: 80px;">
        <div class="content">
            <div class="content_right_newcourse" ms-controller="createCourse">
                <div class="content_right_newcourse_create">
                	<div class="content_right_newcourse_top">
                		<p class="content_right_newcourse_top_num"></p>
                		<p class="content_right_newcourse_top_text">上传</p>
                	</div>
                    <div class="content_right_newcourse_create_con">
                        <div class="content_right_newcourse_create_con_list" ms-repeat-outer="postInfo.prelearnInfo">
                            <div class="courseopen">
                                <div class="content_right_newcourse_create_con_node_li" ms-repeat-inner="outer.nodeInfo">
                                    <div class="content_right_newcourse_create_con_node_li_sort_box" ms-mouseenter = "sortShow()" ms-mouseleave = "sortHide()">
                                        <div class="content_right_newcourse_create_con_node_li_sort">
                                            <p class="content_right_newcourse_create_con_node_li_sort_up" ms-click = "sectionUp(outer.nodeInfo.$model, $index, $outer.$index)"></p>
                                            <p class="content_right_newcourse_create_con_node_li_sort_down" ms-click = "sectionDown(outer.nodeInfo.$model, $index, $outer.$index)"></p>
                                        </div>
                                    </div>
                                    <div class="content_right_newcourse_create_con_li">
                                        <div class="content_right_newcourse_create_con_li_num" ms-html="$index>8?$index+1:'0'+($index+1)">01</div>
                                        <div class="content_right_newcourse_create_con_li_tit"><input type="text" ms-duplex="inner.title" placeholder="请添加小节名称（18字内）" maxlength="18"></div>
                                        <div class="content_right_newcourse_create_con_li_btncon">
                                            <div class="content_right_newcourse_create_con_li_btncon_btn upload" ms-attr-type="1" ms-attr-chapter="$outer.$index" ms-attr-node="$index" ms-slectfile title = "上传您的本地课件资源"></div>
                                            <div class="content_right_newcourse_create_con_li_btncon_btn del" ms-click="delchapter(1,inner,outer,$outer.$index,$index)"></div>
                                        </div>
                                        <div style="clear: both"></div>
                                        <div>
                                            <input class="fileresource" name="fileresource" type="file" class="files" multiple style="display: none">
                                        </div>
                                    </div>
                                    <div>
                                        <div class="content_right_newcourse_create_con_li_li_con" style="height: 46px;" ms-repeat-el="inner.dataInfo">
                                            <div class="content_right_newcourse_create_con_li_tit" style="border: none; width: 558px;height: 46px;padding-left: 60px;">
                                                <div class="content_right_newcourse_create_con_li_tit_li_name_con">
                                                    <div class="content_right_newcourse_create_con_li_tit_li_name_con_name" ms-html="el.title">一去二三里.doc</div>
                                                    <div class="content_right_newcourse_create_con_li_tit_li_name_con_status" ms-if=el.fileID><img
                                                                src="image/newok.png" alt=""></div>
                                                </div>
                                                <div class="content_right_newcourse_create_con_li_li_con_jdbarcon" ms-if="el.showjdbar">
                                                    <div class="content_right_newcourse_create_con_li_li_con_jdbarcon_bar_a">
                                                        <div class="content_right_newcourse_create_con_li_li_con_jdbarcon_bar_b" ms-css-width="[--el.progressBara--]%">
                                                            <div class="content_right_newcourse_create_con_li_li_con_jdbarcon_bar_c" ms-css-width="[--el.progressBarb--]%">
                                                            </div>
                                                        </div>
                                                    </div>
                                                    <div class="content_right_newcourse_create_con_li_li_con_jdnum hide" style="text-indent:10px" ms-class="hide:el.jdmsg">读取中..
                                                    </div>
                                                    <div class="content_right_newcourse_create_con_li_li_con_jdnum hide" ms-class="hide:!el.jdmsg" ms-html="el.progressBarb+'%'">
                                                        75%
                                                    </div>
                                                </div>
                                            </div>
                                            <div class="content_right_newcourse_create_con_li_btncon_btn del" style="height: 26px;" ms-click="dostopupload(el,$outer.$outer.$index,$outer.$index,$index)"></div>
                                            <div style="clear: both"></div>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="content_right_newcourse_create_con_node_li_addbtn" style="border:none">
                                <div class="content_right_newcourse_create_con_node_li_addbtn_l" ms-click="addchapter(2, $index)">添加小节</div>
                            </div>
                            <div style="height: 40px;clear: both;"></div>
                        </div>
                        <input type="text" value="" class="md5container" id="md5container" style="display: none">
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
</html>

<script>
    require(['create'], function (modelb) {
        avalon.directive("slectfile", {
            init: function (binding) {
                var elem = binding.element;
                avalon(elem).bind("click", function () {
                    var type = $(this).attr('type');
                    var chapterIndex = $(this).attr('chapter');  //当前章索引
                    var nodeIndex = $(this).attr('node');        //当前节索引
                    var file;                                    //文件对象
                    $(this).parent().next().html('<input class="fileresource" name="fileresource" type="file" class="files" multiple style="display: none">');
                    var inputf = $(this).parent().next().find('.fileresource');   //input 选择文件框
                    inputf.bind('change', function (e) {
                        file = inputf[0].files;    //获取文件对象
                        //清空文件域
                        inputf.after(inputf.clone().val(""));
                        inputf.remove();
                        var filelength = file.length;
                        if (type == 1) var typearr = modelb.postInfo.prelearnInfo[chapterIndex].nodeInfo[nodeIndex].dataInfo;
                        var perarrlength = typearr.length;
                        if (filelength + perarrlength > 1) {
                            alert('资料数不可超过1个');
                            return false
                        }
                        for (var i = 0; i < filelength; i++) {//格式验证
                            var suffix = file[i].name.substring(file[i].name.lastIndexOf('.') + 1);
                            if (!suffix.match(/(xls|xlsx|doc|docx|ppt|pdf|png|jpg|jpeg|mp3|wma|wav|mov|mp4|flv|avi|rmvb|wmv|mkv|swf)/i)) {
                                alert('文件格式错误');
                                return false
                            }
                            /*if (!modelb.countsize(file[i].name, file[i].size)) {
                             alert(file[i].name + '文件大小超过1G');
                             return false;
                             }*/
                        }
//                        modelb.getDefaultPic(file, filelength);
                        modelb.uploadziliao(file, filelength, chapterIndex, nodeIndex, typearr); //执行上传
                    });

                    inputf.click();
                })
            }
        });

        avalon.scan();
    });
</script>
```
#### JS code
```javascript
define(['PrimecloudPaas', 'ZeroClipboard', 'async'], function (PrimecloudPaas, ZeroClipboard, async) {

    window['ZeroClipboard'] = ZeroClipboard;
    var model = avalon.define({
        $id: 'createCourse',

        //上传接口
        url1: "http://zhijing.com/lessonComment/uploadResource",

        //调转码接口
        url2: "http://zhijing.com/lessonComment/getFileInfo",

        // 提交data
        postInfo: {
            courseId: '',
            baseInfo: {
                teacherId: '',
                courseTitle: ''
            },
            prelearnInfo: [
                {
                    title: '课前导学',
                    courseId: '',
                    nodeIds: [],
                    dataIds: [],
                    nodeInfo: [
                        {
                            sort: 0,
                            title: '',
                            dataInfo: [],
                            isFree: 0,
                            detailTitle: ''
                        }
                    ]
                }
            ]
        },

        //小节排序
        sortShow: function () {
            $(this).children().show();
        },
        sortHide: function () {
            $(this).children().hide();
        },

        //向上移动
        sectionUp: function (arr, index, outerindex) {
            if (index != 0) {
                var sec1 = arr[index];
                var sec2 = arr[index - 1];
                arr.splice(index - 1, 1, sec1);
                arr.splice(index, 1, sec2);
            }
            model.postInfo.prelearnInfo[outerindex].nodeInfo = arr;
        },

        //向下移动
        sectionDown: function (arr, index, outerindex) {
            if (index != arr.length - 1) {
                var sec1 = arr[index];
                var sec2 = arr[index + 1];
                arr.splice(index + 1, 1, sec1);
                arr.splice(index, 1, sec2);
            }
            model.postInfo.prelearnInfo[outerindex].nodeInfo = arr;
        },

        //小节的排序字段赋值
        sortFun: function(){
            for (var i = 0; i < model.postInfo.prelearnInfo.length; i++) {
                for (var j = 0; j < model.postInfo.prelearnInfo[i].nodeInfo.length; j++) {
                    model.postInfo.prelearnInfo[i].nodeInfo[j].sort = j + 1;
                    //console.log(model.postInfo.prelearnInfo[i].nodeInfo[j].sort);
                }
            }
        },

        //添加章节
        addchapter: function (type, index) {
            switch (type) {
                case 2:
                    $(this).parent().siblings('.courseopen').removeClass('hide');
                    $(this).parent().parent().children('.content_right_newcourse_create_con_bar').children('.content_right_newcourse_create_con_bar_img').removeClass('down').addClass('up');
                    model.postInfo.prelearnInfo[index].nodeInfo.push(
                        {
                            sort: 0,
                            title: '',
                            dataInfo: [],
                            isFree: 0,
                            detailTitle: ''
                        }
                    );
                    console.log(model.postInfo);
                    break;
            }
        },

        //文件大小计算
        countsize: function (dataname, datasize) {
            if (datasize > 1024 * 1024 * 1024) {
                return false;
            } else {
                return true;
            }
        },

        //添加资料--添加文件信息到资料数组
        uploadziliao: function (data, datalength, chapterIndex, nodeIndex, typearr) {
            for (var i = 0; i < datalength; i++) {
                var title = data[i].name;
                var courseFormat = data[i].name.substring(data[i].name.lastIndexOf('.') + 1);
                var arrlastindex = typearr.push(
                    {
                        title: title,
                        courseFormat: courseFormat,
                        courseSize: 0,
                        fileID: '',
                        courseTime: 0,
                        convertype: 0,
                        videoType: 0,
                        showjdbar: false,       //进度条显示与隐藏
                        stopupload: false,      //是否点击取消上传
                        jdmsg: '',              //进度条提示信息
                        progressBara: '0',      //读取进度条
                        progressBarb: '0',      //上传进度条
                        isSuccess: 2            //默认转码中
                    }
                );
                var nowIndex = nodeIndex;
                // if()
                model.fordataupload(data[i], typearr[arrlastindex - 1], arrlastindex - 1, nowIndex);
            }
            //上传文件第一个文件名赋值
            if (!model.postInfo.prelearnInfo[chapterIndex].nodeInfo[nodeIndex].title) {
                model.postInfo.prelearnInfo[chapterIndex].nodeInfo[nodeIndex].title = model.postInfo.prelearnInfo[chapterIndex].nodeInfo[nodeIndex].dataInfo[0].title.split('.')[0];
            }
        },

        //请求
        request: function (url, data, callback, ignoreFailed) {
            var method = 'POST';
            if (typeof data === 'function') {
                method = 'GET';
                ignoreFailed = callback;
                callback = data;
                data = null;
            }
            $.ajax({
                url: url,
                dataType: 'json',
                type: method,
                data: data,
                success: function (response) {
                    /*if (!response.type) {
                     if (!ignoreFailed) { return callback(new Error('暂无数据')); }
                     response = false;
                     }*/
                    return callback(null, response);
                },
                //error: callback
                error: function (error) {
                    console.log('网络异常，请重试');
                    alert('网络异常，请重试');
                }
            });
        },

        //循环创建新添加的上传对象，并执行上传
        videoType: 0,
        nodeIndex: 0,
        fordataupload: function (file, dataobj, dataindex, nodeIndex) {
            model.nodeIndex = nodeIndex;
            model.postInfo.prelearnInfo[0].nodeInfo[model.nodeIndex].dataInfo.file = file;
            model.postInfo.prelearnInfo[0].nodeInfo[model.nodeIndex].dataInfo.dataobj = dataobj;
            async.auto({
                step0: model.upload0,
                step1: ["step0", model.upload1],
                step2: ["step1", model.upload2],
                step3: ["step2", model.upload3],
                step4: ["step2", model.upload4]
            }, function(err, results){
                if(err){
                    console.log(err);
                    if(err == "fail"){
                        console.log(model.postInfo.prelearnInfo[0].nodeInfo[results.step0].dataInfo.file.name + ' 上传失败，请重试');
                        alert(model.postInfo.prelearnInfo[0].nodeInfo[results.step0].dataInfo.file.name + ' 上传失败，请重试');
                    }else if(err == "cancel"){
                        model.postInfo.prelearnInfo[0].nodeInfo[results.step0].dataInfo.dataobj.stopupload ? console.log('----------------------------------' + model.postInfo.prelearnInfo[0].nodeInfo[results.step0].dataInfo.file.name + '取消上传----------------------------------') : console.log('----------------------------------code:401----------------------------------');
                    }
                }
                console.log(results);
            });
        },

        // 创客视频终止上传
        stopupload: function (nodeobj) {
            nodeobj.stopupload = true;
            nodeobj.showjdbar = false;
            nodeobj.jdmsg = '';
            nodeobj.progressBara = '0';
            nodeobj.progressBarb = '0';
            if (nodeobj.paas) {
                nodeobj.paas.endUpload();
                nodeobj.paas.endMD5();
                nodeobj.paas = null;
            }
        },

        //执行终止
        dostopupload: function (nodeobj, outoutindex, outindex, index) {
            if (!confirm('确定删除？')) {
                return false;
            }
            model.stopupload(nodeobj);
            model.postInfo.prelearnInfo[outoutindex].nodeInfo[outindex].dataInfo.splice(index, 1);//删除章节信息
            if (nodeobj.hasOwnProperty("id")) {
                // model.deletedatabase(2, nodeobj.id);
                model.postInfo.prelearnInfo[outoutindex].dataIds.push(nodeobj.id);
            }
        },

        // 创客章节删除
        delchapter: function (type, obj, outobj, outindex, index) {

            if (!confirm('确定删除？')) {
                return false;
            }
            setTimeout(function() {
                if (obj.dataInfo) {
                    $.each(obj.dataInfo, function () {
                        model.stopupload(this);//停止上传
                    });
                }
                if (type == 1) {
                    model.postInfo.prelearnInfo[outindex].nodeInfo.splice(index, 1);
                    //如果nodeInfo已删除干净，改变stack的类名为down
                    if (!model.postInfo.prelearnInfo[outindex].nodeInfo.length) {
                        $(this).parent().parent().parent().parent().siblings('.content_right_newcourse_create_con_bar').children('.content_right_newcourse_create_con_bar_img').removeClass('up').addClass('down');
                    }
                    if (obj.hasOwnProperty("id")) {
                        // model.deletedatabase(1, obj.id)
                        model.postInfo.prelearnInfo[outindex].nodeIds.push(obj.id);
                    }
                }
            }, 300)

        },

        //删除已添加信息
        deletedatabase: function (type, id) {
            if(! id) {
                alert("无法删除!");
                return false;
            }
            $.ajax({
                url: '/teacherCourse/deleteCourseChapter/' + type + '/' + id,
                type: 'GET',
                dataType: 'json',
                success: function (response) {
                    //
                }
            });
        },

        //上传分解0
        upload0: function (callback) {
            callback(null, model.nodeIndex);
        },

        //上传分解1
        upload1: function (data, callback) {
            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.paas = new PrimecloudPaas();
            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.paas.MD5(model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file, function (result) {
                //判断是否 点击 取消上传
                if (model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.stopupload) {
                    console.log('----------------------------------' + model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name + ' 取消上传 ----------------------------------');
                    //model.delpdataup(dataobj.dataindex); //从提交对象中删除这条取消的资料信息
                    return false;
                }
                if (result) {
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileName = model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.paas.splitFileName(model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name);
                    $('#md5container').val(result);
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileMD5 = $('#md5container').val();
                    console.log('----------------------------------开始上传----------------------------------');
                    callback(null, result);
                } else {
                    callback("fail");
                }
            }, function (pos, size) {
                if (model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.stopupload) {
                    console.log('----------------------------------' + model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name + ' 取消上传 ----------------------------------');
                    //model.delpdataup(model.dataobj.dataindex); //从提交对象中删除这条取消的资料信息
                    return false;
                }
                //显示进度条
                model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.showjdbar = true;
                //修改进度条进度提示信息
                var jd = parseInt(pos / size * 100);
                model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.progressBara = jd;
                if (jd == 100) {
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.jdmsg = '上传中...';
                    console.log('----------------------------------' + model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name + ': 扫描完成 ----------------------------------');
                }
            });
        },

        //上传分解2
        upload2: function(data, callback){
            model.request(model.url1, {
                md5: model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileMD5,
                filename: model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileName,
                directory: '/'
            }, function(err, response){
                if (!model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.stopupload && response.data.code != 401) {
                    callback(null, response.data.data);
                } else {
                    callback("cancel");
                }
            });
        },

        //上传分解3
        upload3: function(data, callback){
            if (data.step2.AllowUpload == 2) {
                console.log('----------------------------------秒传----------------------------------');
                model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.progressBarb = 100;
                model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.jdmsg = '上传成功！';
                setTimeout(function () {
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileID = data.step2.FileID;
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.courseTime = 3600;
                    //隐藏滚动条
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.showjdbar = false;
                    model.request(model.url2, {fileID: data.step2.FileID}, function(err, response){
                        //转码
                        var convertype;
                        var suffix = model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name.substring(model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name.lastIndexOf('.') + 1).toLowerCase();
                        if (suffix.match(/(mp4|flv|avi|rmvb|wmv|mkv|mov)/i)) {
                            convertype = 0;
                        } else if (suffix.match(/(xls|xlsx|doc|docx|pdf|ppt)/i)) {
                            convertype = 2;
                        } else if (suffix.match(/(mp3|wma|wav)/i)) {
                            convertype = 1;
                        } else {
                            convertype = 3;
                        }
                        if(response.data.code == 200 && response.data.data.ExecuteCode == 1) {
                            // console.log(111)
                            // console.log(response.data.data)
                            model.videoType = response.data.data.VideoType;
                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.courseTime = response.data.data.Duration ? Math.round(response.data.data.Duration) : 0;
                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.videoType = response.data.data.VideoType;
                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.convertype = convertype;
                        }
                        //model.request('/lessonComment/transformation', {
                        //    fileID: response.data.data.FileID,
                        //    convertype: convertype,
                        //    videoType: model.videoType
                        //}, function(err, response){
                        //    //
                        //});
                    });
                }, 1000);
            }
            callback();
        },

        //上传分解4
        upload4: function(data, callback){
            if (data.step2.AllowUpload == 1) {
                var uploadData = {
                    url: data.step2.UUrl,
                    method: "POST",
                    data: {
                        filedata: model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file
                    }
                };
                if (data.step2.UploadLength > 0) {
                    uploadData.resume = data.step2.UploadLength;
                    console.log('断点续传');
                    console.log(data.step2);
                    console.log(uploadData);
                }
                model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.paas.requestUpload(uploadData, function (e) {
                    //console.log(e);
                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.progressBarb = parseInt(e.loaded / e.total * 100) ? parseInt(e.loaded / e.total * 100) : 0;
                }, function(e){
                    //console.log(e);
                    var n = 1;
                    async.whilst(
                        function(){
                            return n > 0;
                        },
                        function(cb){
                            model.request(model.url1, {
                                md5: model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileMD5,
                                filename: model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileName,
                                directory: '/'
                            }, function(err, response){
                                if (!model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.stopupload && response.data.code != 401) {
                                    if (response.data.data.AllowUpload == 2) {
                                        console.log('----------------------------------秒传----------------------------------');
                                        model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.progressBarb = 100;
                                        model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.jdmsg = '上传成功！';
                                        setTimeout(function () {
                                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.fileID = response.data.data.FileID;
                                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.courseTime = 3600;
                                            //隐藏滚动条
                                            model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.showjdbar = false;
                                            model.request(model.url2, {fileID: response.data.data.FileID}, function(err, response){
                                                //转码
                                                var convertype;
                                                var suffix = model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name.substring(model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.file.name.lastIndexOf('.') + 1).toLowerCase();
                                                if (suffix.match(/(mp4|flv|avi|rmvb|wmv|mkv|mov)/i)) {
                                                    convertype = 0;
                                                } else if (suffix.match(/(xls|xlsx|doc|docx|pdf|ppt)/i)) {
                                                    convertype = 2;
                                                } else if (suffix.match(/(mp3|wma|wav)/i)) {
                                                    convertype = 1;
                                                } else {
                                                    convertype = 3;
                                                }
                                                if(response.data.code == 200 && response.data.data.ExecuteCode == 1) {
                                                    // console.log(111)
                                                    // console.log(response.data.data)
                                                    model.videoType = response.data.data.VideoType;
                                                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.courseTime = response.data.data.Duration ? Math.round(response.data.data.Duration) : 0;
                                                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.videoType = response.data.data.VideoType;
                                                    model.postInfo.prelearnInfo[0].nodeInfo[data.step0].dataInfo.dataobj.convertype = convertype;
                                                }
                                                //model.request('/lessonComment/transformation', {
                                                //    fileID: response.data.data.FileID,
                                                //    convertype: convertype,
                                                //    videoType: model.videoType
                                                //}, function(err, response){
                                                //    //
                                                //});
                                            });
                                        }, 1000);
                                        n = 0;
                                    }
                                }
                                cb();
                            });
                        },
                        function(err){
                            console.log(err);
                        }
                    );
                });
            }
            callback();
        },

    });
    return model;
});

