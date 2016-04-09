unit Unit1;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, ComCtrls, StdCtrls, Filectrl,ShellAPI, Menus;

type
  TForm1 = class(TForm)
    Label1: TLabel;
    Label2: TLabel;
    btnStart: TButton;
    txtSource: TEdit;
    txtDestination: TEdit;
    chkDat: TCheckBox;
    chkIx: TCheckBox;
    chkDia: TCheckBox;
    btnBrowseSource: TButton;
    btnBrowseDestination: TButton;
    ListBox1: TListBox;
    btnScan: TButton;
    lblTotalCount: TLabel;
    lblSize: TLabel;
    DateTimePicker1: TDateTimePicker;
    chkLastModified: TCheckBox;
    MainMenu1: TMainMenu;
    File1: TMenuItem;
    Exit1: TMenuItem;
    Help1: TMenuItem;
    About1: TMenuItem;
    procedure FormCreate(Sender: TObject);
    procedure btnStartClick(Sender: TObject);
    procedure btnStopClick(Sender: TObject);
    procedure btnBrowseSourceClick(Sender: TObject);
    procedure btnBrowseDestinationClick(Sender: TObject);
    procedure btnScanClick(Sender: TObject);
    procedure Button1Click(Sender: TObject);
    procedure chkDatClick(Sender: TObject);
    procedure chkLastModifiedClick(Sender: TObject);
    procedure About1Click(Sender: TObject);
    procedure Exit1Click(Sender: TObject);

  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  Form1: TForm1;
  isNewScanMode : BOOL;

implementation

uses Math, StrUtils;

{$R *.dfm}

function FileOperation(ASource,ADest:string):Boolean;
var
P:_SHFILEOPSTRUCT;
begin
ASource:=ASource+ #0;
ADest:=ADest+ #0;
with P do
begin
Wnd:=Application.Handle;
wFunc:=FO_COPY;
pFrom:=PChar(ASource);
pTo:=PChar(ADest);
fFlags:=FOF_NOCONFIRMATION;
end;
SHFileOperation(P);

Result:=not P.fAnyOperationsAborted;
end;

//---------------------------------
function ScanDir(filetype:String;  SourceDir:string):TStringList;

var
SearchResult: TSearchRec;
List1 : TStringList;
str:string;
begin

List1 := TStringList.Create;

 if FindFirst('c:\Retail\rpro\*.dat', faAnyFile, searchResult) = 0 then
  begin
    repeat

   //str :=  searchResult.Name ;
   str := searchResult.name;
   List1.Add(str);
    until FindNext(searchResult) <> 0;

    // Must free up resources used by these successful finds
    FindClose(searchResult);
  end;

result:= List1;
list1.Free;
end;


procedure TForm1.FormCreate(Sender: TObject);
begin

txtSource.Text := '';
txtDestination.Text := '';
btnStart.Enabled := False;
txtDestination.Enabled := False;
DateTimePicker1.Enabled:= False;
btnBrowseDestination.Enabled:= False;



isNewScanMode:=False;


lblTotalCount.Caption := '';
lblSize.Caption := '';
txtSource.Text:= 'C:\Retail\Rpro';

end;

procedure TForm1.btnStartClick(Sender: TObject);
var
i : Integer;
FilesList:String;
begin

if txtSource.Text = ''  then
begin
MessageDlg('Provide Source Path and try again.',mtWarning, mbOKCancel  ,0 );
Exit;
end;

if txtDestination.Text  = ''  then
begin
MessageDlg('Provide Destination Path and try again.',mtWarning, mbOKCancel  ,0 );
Exit;
end;



  btnStart.Enabled:=False;

for i:= 0 to ListBox1.Count-1
do
//FileOperation( txtSource.Text  + '\' + ListBox1.items[i]  , txtDestination.Text);
FilesList := FilesList + txtSource.Text  + '\' + ListBox1.items[i] + #0;
FilesList := FilesList + #0 + #0;
FileOperation( FilesList    , txtDestination.Text);
ShowMessage('Copy Completed');
btnStart.Enabled := True;
end;

//FileOperation( txtSource.Text  + '\*.dia'    , txtDestination.Text);

procedure TForm1.btnStopClick(Sender: TObject);
begin
  btnStart.Enabled:=True;

end;

procedure TForm1.btnBrowseSourceClick(Sender: TObject);

var
fileDialog : TOpenDialog;
Dir : String;

begin
SelectDirectory('Select a directory', '', Dir);

if Dir <> '' then
  txtSource.Text := Dir;

end;

procedure TForm1.btnBrowseDestinationClick(Sender: TObject);
var
Dir: String;
begin

SelectDirectory('Select Destination Directory','',Dir);
if Dir <> '' then
txtDestination.Text := Dir;


end;

procedure TForm1.btnScanClick(Sender: TObject);
var
SourceDir: string;
filetype:string;
SearchResult: TSearchRec;
cnt: Integer;
filesize: Double;
filedate: TDate;
intFS: Integer;
dt : TDateTime;

begin
{Clear List Box}

if txtSource.Text = '' then
begin
 ShowMessage('Plese provide source directory and try again.');
 exit;
end;

if isNewScanMode = False then
begin
btnScan.Caption := 'New Scan';
isNewScanMode := True;

btnStart.Enabled := True;
chkDat.Enabled := false;
chkIx.Enabled := false;
chkDia.Enabled := false;
txtDestination.Enabled :=true;
btnBrowseDestination.Enabled := True;
chkLastModified.Enabled := False;
DateTimePicker1.Enabled := False;
txtSource.Enabled:= False;
btnBrowseSource.Enabled:=False;


end
else
begin
btnScan.Caption := 'Scan Source Dir';
isNewScanMode := False;

btnStart.Enabled := false;
chkDat.Enabled := true;
chkIx.Enabled := true;
chkDia.Enabled := true;
txtDestination.Enabled :=false;
btnBrowseDestination.Enabled := false;
chkLastModified.Enabled := true;
DateTimePicker1.Enabled := true;
txtSource.Enabled:= true;
btnBrowseSource.Enabled:=true;
exit;

end;


ListBox1.Clear;
cnt:=0;
filesize:=0;

SourceDir := txtSource.Text;

DateTimePicker1.Time := StrToTime('00:00:00');
dt := DateTimePicker1.Datetime;

{Read Data Files}
//------------------------------

if chkDat.Checked = True then
begin
filetype:= '*.DAT';

if FindFirst(txtSource.Text + '\' + filetype, faAnyFile, searchResult) = 0 then
  begin
    repeat

      if LeftStr(searchResult.Name,3) <> 'LOG' then
       begin

       if not chkLastModified.Checked then
        begin
         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
        end
       else if  SearchResult.Time >=  DateTimeToFileDate(dt)  then
         begin

         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
         end;

        end;

    until FindNext(searchResult) <> 0;
    // Must free up resources used by these successful finds
    FindClose(searchResult);
  end;
end;
{Read Index Files}
//-------------------------------

filetype:= '*.ix';

if chkIx.Checked = True then
begin
if FindFirst(txtSource.Text + '\' + filetype, faAnyFile, searchResult) = 0 then
  begin
    repeat
      if LeftStr(searchResult.Name,3) <> 'LOG' then
       begin

       if not chkLastModified.Checked then
        begin
         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
        end
       else if SearchResult.Time >=  DateTimeToFileDate(dt)  then
         begin

         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
         end;

        end;

    until FindNext(searchResult) <> 0;
    // Must free up resources used by these successful finds
    FindClose(searchResult);
  end;
end;


{Read Dia Files}
//-------------------------------

filetype:= '*.dia';

if chkdia.Checked = True then
begin
if FindFirst(txtSource.Text + '\' + filetype, faAnyFile, searchResult) = 0 then
  begin
    repeat

if LeftStr(searchResult.Name,3) <> 'LOG' then
       begin

       if not chkLastModified.Checked then
        begin
         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
        end
       else if SearchResult.Time >=  DateTimeToFileDate(dt)  then
         begin

         ListBox1.Items.Add(searchResult.Name );
         cnt := cnt+1;
         filesize:= filesize + searchResult.Size;
         end;

        end;


    until FindNext(searchResult) <> 0;
    // Must free up resources used by these successful finds
    FindClose(searchResult);
  end;
end;




lblTotalCount.Caption:= 'Total Files :' + IntToStr(cnt);
filesize := filesize/1024/1024;
//filesize := RoundTo(filesize);
lblSize.Caption:= 'Total Size:' + CurrToStr(filesize) + ' MB';


end;


procedure TForm1.Button1Click(Sender: TObject);
var
str : string;

begin

DateTimePicker1.Time:= StrToTime('11:59:00 PM');
str :=TimeToStr(  DateTimePicker1.Time);
ShowMessage(str);

end;

procedure TForm1.chkDatClick(Sender: TObject);
begin

//TForm1.btnScanClick;

end;

procedure TForm1.chkLastModifiedClick(Sender: TObject);
begin

If chkLastModified.Checked then
    DateTimePicker1.Enabled:=true
else
    DateTimePicker1.Enabled:=false;


end;

procedure TForm1.About1Click(Sender: TObject);
var str:String;
begin

str := 'Retail Pro File Copier - ' + chr(13) +chr(13)+ 'By GTM IT-Department' + chr(13) + 'Developed in Delphi 7';
ShowMessage(str);
end;

procedure TForm1.Exit1Click(Sender: TObject);
begin

close;

end;

end.
