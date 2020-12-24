Android 调用系统自带文件浏览器

### 一、获取某个路径的权限

```kotlin
// 选择需要获取权限的目录
fun openDirectory(pickerInitialUri: Uri) {
    // Choose a directory using the system's file picker.
    val intent = Intent(Intent.ACTION_OPEN_DOCUMENT_TREE).apply {
        // Provide read access to files and sub-directories in the user-selected
        // directory.
        flags = Intent.FLAG_GRANT_READ_URI_PERMISSION

        // Optionally, specify a URI for the directory that should be opened in
        // the system file picker when it loads.
        putExtra(DocumentsContract.EXTRA_INITIAL_URI, pickerInitialUri)
    }

    startActivityForResult(intent, your-request-code)
}

// 返回后，对获取的路径进行操作
override fun onActivityResult(
        requestCode: Int, resultCode: Int, resultData: Intent?) {
    if (requestCode == your-request-code
            && resultCode == Activity.RESULT_OK) {
        // The result data contains a URI for the document or directory that
        // the user selected.
        resultData?.data?.also { uri ->
            // Perform operations on the document using its URI.
        }
    }
}

// 保留权限
fun savePermission(uri: Uri){
    val contentResolver = applicationContext.contentResolver

	val takeFlags: Int = Intent.FLAG_GRANT_READ_URI_PERMISSION or
        		   		 Intent.FLAG_GRANT_WRITE_URI_PERMISSION
	
    // Check for the freshest data.
	contentResolver.takePersistableUriPermission(uri, takeFlags)
}

// 移除权限
fun releasePermission(uri: Uri){
    val contentResolver = context.contentResolver
    val takeFlags: Int = Intent.FLAG_GRANT_READ_URI_PERMISSION or 	
    					 Intent.FLAG_GRANT_WRITE_URI_PERMISSION
    contentResolver.releasePersistableUriPermission(uri, takeFlags)
    viewModel.loadPersistedPermissions()
}
```



### 二、获取某个类型的文件

```kotlin
Intent intent = new Intent(Intent.ACTION_GET_CONTENT)
intent.setType("*/*");//设置类型，我这里是任意类型，任意后缀的可以这样写。
intent.addCategory(Intent.CATEGORY_OPENABLE)
startActivityForResult(intent,1);
```

- **类型修改**

  ```kotlin
   intent.setType(“image/*”);
  //intent.setType(“audio/*”); //选择音频
  //intent.setType(“video/*”); //选择视频 （mp4 3gp 是android支持的视频格式）
  //intent.setType(“video/*;image/*”);//同时选择视频和图片
  ```

- **在返回的Activity中获取选择内容**

  ```kotlin
  @Override
  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
      if (resultCode == Activity.RESULT_OK) {//是否选择，没选择就不会继续
          Uri uri = data.getData();//得到uri，后面就是将uri转化成file的过程。
          String[] proj = {MediaStore.Images.Media.DATA};
          Cursor actualimagecursor = managedQuery(uri, proj, null, null, null);
          int actual_image_column_index = actualimagecursor.getColumnIndexOrThrow(MediaStore.Images.Media.DATA);
          actualimagecursor.moveToFirst();
          String img_path = actualimagecursor.getString(actual_image_column_index);
          File file = new File(img_path);
          Toast.makeText(MainActivity.this, file.toString(), Toast.LENGTH_SHORT).show();
      }
  }
  ```

  