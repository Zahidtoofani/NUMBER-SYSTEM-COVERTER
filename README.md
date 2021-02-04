# NUMBER-SYSTEM-COVERTER
"NUMBER SYSTEM CONVERTER"  is  most simple project,  it simply coverts a number of a number sytem to its respective representation in other number systems 
import tkinter as t
import re
def convert_to_print(var):
    dic = {"10":"A","11":"B","12":"C","13":"D","14":"E","15":"F"}
    fnl=[]
    for i in var:
        if i=="10" or i=="11" or i=="12" or i=="13" or i=="14" or i=="15" :
            fnl.append(dic[i])
        else:
            fnl.append(i)
    return(fnl)
def convert_to_cal(var):
    dic={"A":"10","B":"11","C":"12","D":"13","E":"14","F":"15",
         "a":"10","b":"11","c":"12","d":"13","e":"14","f":"15"}
    fnl=[]
    for i in var:
        if i == "A" or i == "B" or i == "C" or i == "D" or i == "E" or i == "F" or i == "a" or i == "b" or i == "c" or i == "d" or i == "e" or i == "f":
            fnl.append(dic[i])
        else:
            fnl.append(i)
    return(fnl)

def __checkInput__(_str, _type):
    _bin = re.compile(r'[^0-1]')
    _oct = re.compile(r'[^0-7]')
    _dec = re.compile(r'[^0-9]')
    _hdec = re.compile(r'[^0-9a-fA-F]')

    if _type == "Binary" or _type == "   Binary   ":
        ans = _bin.search(_str)
        if ans:
            return False
    elif _type == "   Octal    ":
        ans = _oct.search(_str)
        if ans:
            return False
    elif _type == "  Decimal   ":
        ans = _dec.search(_str)
        if ans:
            return False
    elif _type == "Hexa Decimal":
        ans = _hdec.search(_str)
        if ans:
            return False
    return True
class convert:
    def __init__(self):
        self.stk = []
        self.dec = 0
    def __DcmlToBnOctl__(self, data, base):
        #print(data)
        #num = int(data)
        num = data
        while num >= base:
            #print('num', num, 'base', base)
            self.stk.append(num % base)
            num = num // base
        self.stk.append(num % base)
        ans = ''
        for i in range(len(self.stk)):
            ans += str(self.stk.pop())
        return ans
    def __DcmlToHexDcml__(self, data):
        num = data
        self.stk = []
        while num > 15:
            var_1 = num % 16
            self.stk.append(str(var_1))
            num = num // 16
        self.stk.append(str(num))
        self.stk = convert_to_print(self.stk)
        ans = ''
        for i in range(len(self.stk)):
            ans += str(self.stk.pop())
        return ans
    def __BnOctlToDcml__(self, data, base):
        data = str(data)
        cntr = len(data)
        for i in data:
            cntr-=1
            temp = int(i)
            self.dec += temp * (base ** cntr)
        return self.dec
    def __HexDcmlToDcml__(self, data):
        self.dec = 0
        data = str(data)
        data = convert_to_cal(data)
        cntr = len(data)
        #print(data)
        for i in data:
            cntr-=1
            temp = int(i)
            self.dec += temp * (16**cntr)
        return self.dec
class GUI:
    def __init__(self):
        self.mainWin = t.Tk()
        self.mainWin.title('BASE CONVERTER')
        self.mainWin.geometry('370x590')
        self.mainWin.resizeable = False
        
    def __frame1__(self):
        frame1 = t.Frame(self.mainWin, width = 370, height = 95, bg = 'lightblue')
        frame1.grid_propagate(0)
        name = t.Label(frame1, text = 'BASE\nCONVERTER', font = 'Corrier 27 bold',
                       bg = 'lightblue', fg = 'white', padx = 9)
        _help = t.Button(frame1, text = ' Help! ', bg = 'moccasin', width = 15) 
        name.grid(row = 0, column = 0)
        _help.grid(row = 0, column = 1)
        frame1.grid(row = 0, column = 0)
    def __frame2__(self):
        frame2 = t.Frame(self.mainWin, bg = 'lightgray',width = 370, height = 90)
        frame2.grid_propagate(0)


        optionVar = t.StringVar()
        optionVar.set("   Binary  ")
        label_base = t.Label(frame2, fg = 'white',bg = 'lightgray', font = 'Times 22', padx = 25, 
                           pady = 10, text = '  Convert from  : ')

        option_menu = t.OptionMenu(frame2, optionVar,
                                        "   Binary   ",
                                        "   Octal    ",
                                        "  Decimal   ",
                                        "Hexa Decimal", command = self.__getBase__)
        option_menu.config(width = 12)
        convert = t.Button(frame2, text = 'Convert', bg = 'moccasin', width = 15,
                           command = self.__getInput__)
        #input widget
        self._input = t.Entry(frame2, width = 30)
        label_base.grid(row = 0, column = 0)
        option_menu.grid(row = 0, column = 1)
        self._input.grid(row = 1, column = 0)
        convert.grid(row = 1, column = 1)
        self.base = 'Binary'
        frame2.grid(row = 1, column = 0)
    def __frame3__(self):
        frame3 = t.Frame(self.mainWin, bg = 'gray',width = 370, height = 370)
        frame3.pack_propagate(0)

        l_binary = t.Label(frame3, text = 'Binary Equivalent :', font = 'Times 20',
                         bg = 'gray', pady = 3)
        l_octal = t.Label(frame3, text = 'Octal Equivalent :', font = 'Times 20',
                        bg = 'gray', pady = 3)
        l_decimal = t.Label(frame3, text = 'Decimal Equivalent :', font = 'Times 20',
                          bg = 'gray', pady = 3)
        l_hex_decimal = t.Label(frame3, text = 'Hexa Decimal Equivalent :', font = 'Times 20',
                              bg = 'gray', pady = 3)
        self.l_error = t.Label(frame3, text = "Input Error", fg = 'red', bg = 'gray',font = 'Times 20')
        self.l_binary_ans = t.Label(frame3, font = 'Times 20',fg = 'blue', bg = 'gray')
        self.l_octal_ans = t.Label(frame3, font = 'Times 20',fg = 'blue', bg = 'gray')
        self.l_decimal_ans = t.Label(frame3, font = 'Times 20',fg = 'blue', bg = 'gray')
        self.l_hex_decimal_ans = t.Label(frame3, font = 'Times 20',fg = 'blue', bg = 'gray')


        l_binary.pack()
        self.l_binary_ans.pack()
        l_octal.pack()
        self.l_octal_ans.pack()
        l_decimal.pack()
        self.l_decimal_ans.pack()
        l_hex_decimal.pack()
        self.l_hex_decimal_ans.pack()
        
        frame3.grid(row = 2, column = 0)
    def __frame4__(self):
        l = t.Label(self.mainWin, text = 'Zahid Ahmad Allaie\nzahidtoofani83406@gmail.com')
        l.grid(row = 3, column = 0)
    def __getBase__(self, base):
        self.base = base
    def __getInput__(self):
        t = self._input.get()
        '''print('Given Number :', t)
        print('It is of type :', self.base)'''
        self.__manager__(t, self.base)
    def __manager__(self, num, base):
        var = convert()
        true = __checkInput__(num, base)
        if true == False:
            self.l_binary_ans.config(text = 'N/A')
            self.l_octal_ans.config(text = 'N/A')
            self.l_decimal_ans.config(text = 'N/A')
            self.l_hex_decimal_ans.config(text = 'N/A')
            self.l_error.config(text = "Input Error")
            self.l_error.pack()
            return
        self.l_error.config(text = "")
        if base == "Binary" or base == "   Binary   ":
            #BINARY To OTHERS
            self.l_binary_ans.config(text = '0')
            dec = var.__BnOctlToDcml__(num, 2)
            #print('dec', dec)
            _oct = var.__DcmlToBnOctl__(dec, 8)
            #print('_oct', _oct)
            hDec = var.__DcmlToHexDcml__(dec)
            #print('hDec', hDec)
            if dec > 0:
                if len(num) > 25:
                    temp = num[:25] + '\n' + num[25:]
                    self.l_binary_ans.config(text = temp)
                else:
                    self.l_binary_ans.config(text = num)
            self.l_octal_ans.config(text = _oct)
            self.l_decimal_ans.config(text = str(dec))
            self.l_hex_decimal_ans.config(text = hDec)
            
        elif base == "   Octal    ":
            #Octal to Others'
            self.l_octal_ans.config(text = '0')
            dec = var.__BnOctlToDcml__(num, 8)
            #print('dec', dec)
            _bin = var.__DcmlToBnOctl__(dec, 2)
            #print('_oct', _oct)
            hDec = var.__DcmlToHexDcml__(dec)
            #print('hDec', hDec)
            if len(_bin) > 25:
                temp = _bin[:25] + '\n' + _bin[25:]
                self.l_binary_ans.config(text = temp)
            else:
                self.l_binary_ans.config(text = _bin)
            if dec > 0:
                self.l_octal_ans.config(text = num)
            self.l_decimal_ans.config(text = str(dec))
            self.l_hex_decimal_ans.config(text = hDec)
            
        elif base == "  Decimal   ":
            #Decimal To Others
            self.l_decimal_ans.config(text = '0')
            if num:
                num = int(num)
            else:
                num = 0
            _bin = var.__DcmlToBnOctl__(num, 2)
            #print('_bin', _bin)
            _oct = var.__DcmlToBnOctl__(num, 8)
            #print('_oct', _oct)
            hDec = var.__DcmlToHexDcml__(num)
            #print('hDec', hDec)
            if len(_bin) > 25:
                temp = _bin[:25] + '\n' + _bin[25:]
                self.l_binary_ans.config(text = temp)
            else:
                self.l_binary_ans.config(text = _bin)
            if num > 0:
                self.l_decimal_ans.config(text = str(num))
            self.l_octal_ans.config(text = _oct)
            self.l_hex_decimal_ans.config(text = hDec)
            
        elif base == "Hexa Decimal":
            #hexadecimal to others
            self.l_hex_decimal_ans.config(text = '0')
            dec = var.__HexDcmlToDcml__(num)
            #print('dec', dec)
            _bin = var.__DcmlToBnOctl__(dec, 2)
            #print('_bin', _bin)
            _oct = var.__DcmlToBnOctl__(dec, 8)
            #print('_oct', _oct)
            if len(_bin) > 25:
                temp = _bin[:25] + '\n' + _bin[25:]
                self.l_binary_ans.config(text = temp)
            else:
                self.l_binary_ans.config(text = _bin)
            if dec > 0:
                self.l_hex_decimal_ans.config(text = num.upper())
            self.l_decimal_ans.config(text = str(dec))
            self.l_octal_ans.config(text = _oct)
        
def main():
    bc = GUI()
    bc.__frame1__()
    bc.__frame2__()
    bc.__frame3__()
    bc.__frame4__()
    bc.mainWin.mainloop()

if __name__ == '__main__':
    main()
