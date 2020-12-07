function getFolders() {
 //調べたいフォルダのidを下に入れる 
 const folder = DriveApp.getFolderById('調べたいフォルダーIDを入力');
 const childFolders = folder.getFolders();
 console.log(folder.getName());
 console.log(childFolders);
 
 let foldersArray = [];
 
 while (childFolders.hasNext()) {
   const childFolder       = childFolders.next();
   const childFolderName   = childFolder.getName();
   const childFolderId     = childFolder.getId();
   const innerChildFolder  = DriveApp.getFolderById(childFolderId);//2019_01など
   const innerChildFolders = innerChildFolder.getFolders();
     
     while(innerChildFolders.hasNext()){
       const targetFolder      = innerChildFolders.next();
       const targetFolderName  = targetFolder.getName();
       const targetFolderId    = targetFolder.getId();
       const targetFolderUrl   = 'https://drive.google.com/drive/u/0/folders/' + targetFolderId;
       foldersArray.push([childFolderName, targetFolderName, targetFolderId, targetFolderUrl]);
     }//inner_while
 }//while
 
 foldersArray.unshift(['親フォルダ', '子フォルダ','フォルダID','URL'])
 console.log(foldersArray);
 
 //const spreadsheet = SpreadsheetApp.getActiveSpreadsheet();
  const ss = SpreadsheetApp.openById("スプシのID");
  const sheet = ss.getSheetByName('シートの名前');
 sheet.getRange(1, 1, foldersArray.length, foldersArray[0].length).setValues(foldersArray);
 
}
