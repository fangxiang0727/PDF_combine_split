# PDF_combine_split
combine and spit PDF
# merge and split pdf

from os import listdir, getcwd
from PyPDF2 import PdfFileReader, PdfFileWriter

#----------------------------------------------------------
def merge_pdfs(paths, output):
    pdf_writer = PdfFileWriter()

    for path in paths:
        pdf_reader = PdfFileReader(path)
        for page in range(pdf_reader.getNumPages()):
            # 把每张PDF页面加入到这个可读取对象中
            pdf_writer.addPage(pdf_reader.getPage(page))

    # 把这个已合并了的PDF文档存储起来
    with open(output, 'wb') as out:
        pdf_writer.write(out)

#-----------------------------------------------------------
def list_all_pdfs():
    #将当前文件夹中所有的PDF文件枚举出来做成列表
    xlist=listdir(getcwd())
    pdflist=[]
    for ele in xlist:
        if '合并成功' not in ele and '.pdf' in ele:
            pdflist.append(ele)

    #按照数字大小将文件名字做成顺序列表，方便后续按照数字顺序逐个合并文件。
    def takeNo(elem):
        x=elem.split('.')
        return int(x[0])
    pdflist.sort(key=takeNo)#升序排列文件名称
    
    return pdflist

#-----------------------------------------------------------
def split_pdf(path):
    pdf = PdfFileReader(path)
    for page in range(pdf.getNumPages()):
        pdf_writer = PdfFileWriter()
        pdf_writer.addPage(pdf.getPage(page))

        output = f'{page}{0}.pdf'
        with open(output, 'wb') as output_pdf:
            pdf_writer.write(output_pdf)

#------------------------------------------------------------
if __name__ == '__main__':
    mode_selection=input('模式选择<C/S>, C代表合并操作，S 代表拆分成单页：\n').upper()
    if mode_selection=='C':
        print('程序会按照数字顺序逐个拼接PDF文档，并输出 合并成功.pdf 作为最终文档')
        paths =list_all_pdfs()
        print(paths)
        merge_pdfs(paths, output='合并成功.pdf')
        input('合并完毕，回车退出')
    if mode_selection=='S':
        print('程序会将指定的pdf文件拆分到单页，并以数字命名')
        path =input('输入需要拆分的PDF文件名字，包括后缀，例如 XXX.pdf，回车确认\n')
        split_pdf(path)
        input('拆分完毕，回车退出')
