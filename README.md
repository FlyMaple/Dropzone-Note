# Dropzone

Dropzone 有隱性創建的功能，會在 contentLoaded 的時後將所有 classname 為 dropzone 的 HTML DOM 做初始化動作

# Options
* Dropzone.autoDiscover: boolean (default: true)，控制自動初始化  
    * ( 若要控制指定的 DOM 不要自動初始化: `Dropzone.options.myAwesomeDropzone = false` )

* url: 檔案上傳的後端處理網址

* method: 檔案上傳的 http method ( ex. get post patch put delete )

* parallelUpload: <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>

* maxFilesize: 上傳檔案最大 size ( MB )

* filesizeBase: 1000，檔案加權值 ( ex. maxFilesize:2 => 2 * 1000 = 1000 bytes )

* paramName: 'testxxx'，Send Request Payload
	``` 
	------WebKitFormBoundaryLZ8XOfSRfQJFlcKd
	Content-Disposition: form-data; name="testxxx"; filename="Chrysanthemum.jpg"
	Content-Type: image/jpeg
	
	
	------WebKitFormBoundaryLZ8XOfSRfQJFlcKd--
	```
* uploadMultiple: 預設上傳 n 個檔案就會有 n 條 Request，若將這參數設為 true，會改用一條 request 傳送
	```
	------WebKitFormBoundaryDirvASxBBCJzKB6z
	Content-Disposition: form-data; name="testxxx[0]"; filename="Chrysanthemum.jpg"
	Content-Type: image/jpeg
	
	
	------WebKitFormBoundaryDirvASxBBCJzKB6z
	Content-Disposition: form-data; name="testxxx[1]"; filename="Koala.jpg"
	Content-Type: image/jpeg
	
	
	------WebKitFormBoundaryDirvASxBBCJzKB6z--
	```
* headers: 幫 request 加上額外的 headers
    * 預設為:  
		```
		headers = {
		   "Accept": "application/json",
		   "Cache-Control": "no-cache",
		   "X-Requested-With": "XMLHttpRequest"
		};    
		```
    * 加上自己的:
		```
		headers = {
		   aaa: 'xxx'
		};    
		```
* addRemoveLinks: 上傳成功後，是否顯示移除按鈕 <span style="background: yellow; color: #000; padding: 0 2px;">好像還有其他用途</span>

* previewsContainer: 自訂上傳後的顯示的預覽 DOM  容器 ( default: null ) 

* hiddenInputContainer: 初始化後的 input file 欄位預設 append 於何處 ( default: body )

* clickable: 目前只發現可以讓 Form disable 的效果 <span style="background: yellow; color: #000; padding: 0 2px;">好像還有其他用途</span>

* createImageThumbnails: 是否顯示預覽圖

* maxThumbnailFilesize: 預覽圖大小限制 (MB)

* thumbnailWidth: 預覽圖的寬度，若未設定則會自動計算

* thubmnailHeight: 預覽圖的高度，若未設定則會自動計算

* maxFiles: 設定最大上傳數量，設定為2，上傳4，只有前兩筆會上傳並且呼叫 maxfilesexceeded event，>=2的時候會在 form 上增加 classname="dz-max-files-reached"

* acceptedFiles: 好像用於檔案類型驗證? <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>

* autoProcessQueue: 控制是否自動上傳 ( default: true )，若設為 false 要手動執行 `myDropzone.processQueue();`

* previewTemplate: 有制式的 DOM 階層格式，用於客製自己的預覽圖 DOM 架構
	``` HTML
    <div class="dz-preview dz-file-preview">
       <div class="dz-details">
           <div class="dz-filename"><span data-dz-name></span></div>
           <div class="dz-size" data-dz-size></div>
           <img data-dz-thumbnail />
       </div>
       <div class="dz-progress"><span class="dz-upload" data-dz-uploadprogress></span></div>
       <div class="dz-success-mark"><span>✔</span></div>
       <div class="dz-error-mark"><span>✘</span></div>
       <div class="dz-error-message"><span data-dz-errormessage></span></div>
    </div>
    ```
    * 可以在任何 event 的時候使用 `file.previewElement` 來達到新增修改 DOM 的目的 

* forceFallback: 好像是將 dropzone 的包裝拆開，讓你一步一步去上傳、server 回應，然後檢視問題

# Events
* complete: 圖片載入完成
	 ``` JS
	 Dropzone.options.myAwesomeDropzone = {
	    init: function() {
	        this.on("addedfile", function(file) { alert("Added file."); });
	    }
	 };
	 ```
* dragenter: 拖曳進入

* dragover: 拖曳覆蓋其中，若在範圍內會一值執行

* dragleave: 拖曳離開

* drop: 釋放拖曳檔案

* dragstart: <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>

* dragend: <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>


# Impl
* resize: 處理縮圖的大小縮放
	```
	resize: function (file) {
		return {
			optHeight: 120, /* 透過 thubmnailHeight 算出比例高 */
			optWidth: 120,  /* 透過 thubmnailWidth 算出比例寬 */
			srcHeight: file.height,
			srcWidth: file.width,
			srcX: 120,
			srcY: 0
		};
	}
	```

* renameFilename: 好像是在做任何處理前先將上傳上來的檔名做 rename 處理
   ``` JS
   renameFilename: function (file) {
      return file.name + '_rename';
   }
   ```

* fallback: 搭配 forceFallback 做出相關介面，用於偵錯，提供一些看起來沒啥用的東西:
    * dictDefaultMessage
    * dictFallbackMessage
    * dictFallbackText
    * dictInvalidFileType
    * dictFileTooBig
    * dictResponseError
    * dictCancelUpload
    * dictCancelUploadConfirmation
    * dictRemoveFile
    * dictMaxFilesExceeded

* Dropzone.confirm: <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>

* createThumbnailFromUrl: 好像可以 show 出 server 上所存放的檔案
	``` JS
	myDropzone.createThumbnailFromUrl(file, imageUrl, callback, crossOrigin);
	```

# Functions
* Dropzone.optionsForElement: 將設定的參數取出 ( ex. `Dropzone.options.myAwesomeDropzone = {}` )
   ``` JS
   Dropzone.options.myAwesomeDropzone = {
      maxFileSize: 1
   };
   ```

* init: 初始化，所有 event 監聽皆實做於此

* accept: <span style="background: red; color: #fff; padding: 0 2px;">unknown</span>




* removeFile: 移除單檔，`myDropzone.removeFile(file);`

* removeAllFiles: 全部移除，`myDropzone.removeAllFiles(boolean);`，參數為 true 的話，若正在上傳的檔案會終止

* processQueue: 執行手動上傳

* files: 全部的檔案列表，`myDropzone.files`，這是一個陣列，存放 File Object

* getAcceptedFiles

* getRejectedFiles

* getQueuedFiles

* getUploadingFiles

* disable

* enable
