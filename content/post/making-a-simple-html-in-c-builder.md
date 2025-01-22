+++
title = "Making a simple html editor in C++ Builder"
date = "2010-09-09"
tags = [
    "c++",
    "c++ builder",
    "windows api",
    "code",
]
thumbnail = "images/posts/cpp_builder.png"
featured = false
codeMaxLines = 100 
codeLineNumbers = true
+++

![](/images/posts/cpp_builder.png "")

Back in the day, I had to make a simple html editor. This can be accomplished in a few lines of code, by using a 
simple TCppWebBrowser (wb) component and the IHTMLDocument2 interface. You will also need a TMemo (memo1) control. 
Here is the source code. I hope it helps someone 🙂

```c++
//---------------------------------------------------------------------------
//author: Adrián Deccico - http://adrian.org.ar
//---------------------------------------------------------------------------
 
#include <vcl.h>
#pragma hdrstop
#include "mshtml.h"
 
#include "Unit1.h"
//---------------------------------------------------------------------------
#pragma package(smart_init)
#pragma link "SHDocVw_OCX"
#pragma resource "*.dfm"
TForm1 *Form1;
//---------------------------------------------------------------------------
__fastcall TForm1::TForm1(TComponent* Owner)
        : TForm(Owner)
{
}
//---------------------------------------------------------------------------
void __fastcall TForm1::btnNavigateAndEditClick(TObject *Sender)
{
        wb->Navigate((WideString)"www.google.com");
        while (wb->Busy)
                Application->ProcessMessages();
 
        if (wb->Document)
        {
                IHTMLDocument2 *html;
                wb->Document->QueryInterface<ihtmldocument2>(&html);
                html->put_designMode(L"On");
                html->Release();
        }
}
//---------------------------------------------------------------------------
void __fastcall TForm1::btnInsertImageClick(TObject *Sender)
{
    if (wb->Document)
    {
          IHTMLDocument2 *html;
          wb->Document->QueryInterface<ihtmldocument2>(&html);
          VARIANT var;
          VARIANT_BOOL receive;
          html->execCommand(L"InsertImage",true,var, &receive);
          html->Release();
    }
}
//---------------------------------------------------------------------------
void __fastcall TForm1::btnGetHtmlClick(TObject *Sender)
{
        if (wb->Document)
        {
                IHTMLDocument2 *html;
                wb->Document->QueryInterface<ihtmldocument2>(&html);
                IHTMLElement *pElement;
                html->get_body(&pElement);
                pElement->get_parentElement(&pElement);
                wchar_t *tmp;
                pElement->get_outerHTML(&tmp);
                Memo1->Lines->Text = tmp;
                pElement->Release();
                html->Release();
        }
}
//---------------------------------------------------------------------------
```