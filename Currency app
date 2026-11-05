import tkinter as tk
import requests,subprocess,json
from datetime import datetime,timedelta
import threading

class CurrencyApp:
    def __init__(self,root):
        self.root=root
        self.root.title('USD to INR Live Rate')
        self.root.geometry('500x280')
        self.root.configure(bg='white')

        self.show_btn=tk.Button(self.root,text='Show',font=('Arial',12,'bold'),bg="#e76b6b",fg='white',width=10,
                                command=lambda: threading.Thread(target=self.get_rate,daemon=True).start())

        self.show_btn.pack(pady=10)

        self.status_var=tk.StringVar(value='.....')
        self.rate_var=tk.StringVar(value='....')
        self.increase_var=tk.StringVar(value='...')
        self.change_var=tk.StringVar(value='..')
        self.time_var=tk.StringVar(value='.')

        frame1=tk.Frame(root,bg='white')
        frame1.pack(fill='x',padx=20,pady=10)
        tk.Label(frame1,text='Status :',font=('Arial',12),width=22,anchor='w',bg='white').pack(side='left')
        tk.Label(frame1,textvariable=self.status_var,font=('Arial',12),bg='white').pack(side='left')


        frame2=tk.Frame(root,bg='white')
        frame2.pack(fill='x',padx=20,pady=6)
        tk.Label(frame2,text='Current rate of INR  :',font=('Arial',12),bg='white',width=22,anchor='w').pack(side='left')
        tk.Label(frame2,textvariable=self.rate_var,font=('Arial',12),bg='white').pack(side='left')

        frame3=tk.Frame(root,bg='white')
        frame3.pack(fill='x',padx=20,pady=6)
        tk.Label(frame3,text='Increase/decrease by :',font=('Arial',12),bg='white',width=22,anchor='w').pack(side='left')
        tk.Label(frame3,textvariable=self.increase_var,font=('Arial',12),bg='white').pack(side='left')
        
        frame4=tk.Frame(root,bg='white')
        frame4.pack(fill='x',padx=20,pady=6)
        tk.Label(frame4,text='Rate change :',font=('Arial',12),bg='white',width=22,anchor='w').pack(side='left')
        tk.Label(frame4,textvariable=self.change_var,font=('Arial',12),bg='white').pack(side='left')

        frame5=tk.Frame(root,bg='white')
        frame5.pack(fill='x',padx=20,pady=6)
        tk.Label(frame5,text='Rate of time :',font=('Arial',12),bg='white',width=22,anchor='w').pack(side='left')
        tk.Label(frame5,textvariable=self.time_var,font=('Arial',12),bg='white').pack(side='left')


    def get_rate(self):
        try:
            today_data= json.loads(subprocess.check_output(['curl','-k','-s','https://open.er-api.com/v6/latest/USD'],timeout=30).decode())
            today_rate=today_data['rates']['INR']

            yesterday=(datetime.now() - timedelta(days=1)).strftime('%Y-%m-%d')
            yest_data= json.loads(subprocess.check_output(['curl','-k','-s',f'http://cdn.jsdelivr.net/npm/@fawazahmedO/currency-api@{yesterday}/v1/currencies/usd.json'],timeout=30).decode())
            yest_rate=yest_data['usd']['inr']

            diff=today_rate-yest_rate
            percent= (diff/yest_rate)*100

            self.status_var.set('success')
            self.rate_var.set(f'{today_rate:.4f}')  
            self.increase_var.set(f'{diff:.4f}')
            self.change_var.set(f'{percent:.2f}%')
            self.time_var.set(datetime.now().strftime('%I:%M%p'))


        except Exception as e:
            print('Error Reason:',e)
            self.status_var.set('No Internet')
            self.rate_var.set('....')
            self.increase_var.set('...')
            self.change_var.set('..')
            self.time_var.set('.')      

        
        






if __name__=='__main__':
    win=tk.Tk()
    app=CurrencyApp(win)
    win.mainloop()
