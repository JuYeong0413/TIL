## Custom Form에 placeholder 추가하기  
```python
class MyCustomSignupForm(SignupForm):
    name = forms.CharField(max_length=10, label='이름', widget=forms.TextInput(attrs={'placeholder': '이름'}))
```

