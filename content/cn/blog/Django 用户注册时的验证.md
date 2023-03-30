---
categories: Django
title: Django Django 用户注册时的验证
tags: [教程,Python]
date: 2023-03-30
lastmod: 2023-03-30 
slug: mpnavlt
thumbnail: https://s1.vika.cn/space/2023/03/30/cbafbe3be9e84e2bbf2c795e32e349ad
published: "true"
---

>《Python 程序语言·Django》读书笔记之十

## 1 方法一：在ModelForm创建时设置  

```python
class LoginModelForm(forms.ModelForm):
    class Meta:
        model = User
        fields = {'username','password'}
        widgets = {
            'username':forms.widgets.TextInput(
                attrs={
                    'class':'form-control',
                    'placeholder':'输入用户名',
                    'id':'username'
                }),
            'password':forms.widgets.PasswordInput(
                attrs = {
                    'class':'form-control',
                    'placeholder':'输入密码',
                    'id':'password'
                }
            )
        }
    def clean_username(self):
        username = self.cleaned_data['username']
        if re.match('1[3-9][0-9]{9}$',username):
            return username
        else:
            raise ValidationError('用户名应为手机号')  

    def clean_password(self):
        password = self.cleaned_data['password']
        if re.match('^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z]{8,18}$',password):
            return password
        else:
            raise ValidationError('密码强度不够')
```  

username验证：username必须是11位手机号码。
password验证：password必须是8-18位，必须且只能包括数字、小写字母和大写字母。
views.py：由于ModelForm与模型绑定，模型字段定义时的验证也要执行。username字段在定义时，有unique属性，因此 infos.is_valid() 语句在 if not User.objects.filter(username = username)后执行，否则，一旦username重复，就会出错。  

```python
def registerView(request):
    if request.method == 'POST':
        infos = LoginModelForm(data= request.POST)
        data = infos.data
        username = data['username']
        password = data['password']
        if not User.objects.filter(username = username):
            if infos.is_valid():
                d = dict(username=username,password=password,is_staff=1,is_active=1)
                user = User.objects.create_user(**d)
                user.save()
                return redirect(reverse('login:login'))
            else:
                error_msg = json.loads(infos.errors.as_json())
                username_error_msg =''
                password_error_msg = ''
                for k in error_msg:
                    if k == 'username': 
                        for error in error_msg['username']:
                            username_error_msg += error['message']
                    if k == 'password':
                        for error in error_msg['password']:
                            password_error_msg += error['message']
        else:
            username_error_msg = '用户名重复'
    title = '手机号注册'
    submitbtn = '注册'
    infos = LoginModelForm()
    tip = '已有用户名？请登录'
    return render(request,'userlogin/login.html',locals())
```  

## 2 方法二：在前端页面，用Jquery事件 

在创建ModelForm时，username 的 id='username'，password的id='password'。  

```js
$("#username").blur(function(){
        var regUsername = /1[3-9][0-9]{9}$/
        if(regUsername.test($("#username").val())){
            $("#username_error_msg").html('')
        }else{
            $("#username_error_msg").html('用户名为手机号吗')
        }
    })
    $("#password").keyup(function(){
        var regPassword = /(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z]{8,18}$/
        if(regPassword.test($("#password").val())){
            $("#password_error_msg").html('')
        }else{
            $("#password_error_msg").html('密码为8-18位，包括数字、大写字母和小写字母')
        }
    })
```