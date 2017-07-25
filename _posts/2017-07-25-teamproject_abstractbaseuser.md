---
title: "AbstractBaseUser"
layout: post
date: 2017-07-24
image: /assets/images/docker_profile.png
headerImage: true
tag:
- TeamProject
star: true
category: blog
author: YM
description: TeamProject User Model Using AbstractBaseUser
---

## AbstractBaseUser 상속

팀프로젝트를 진행하면서 User 모델을 만드는데 많은 시간을 삽질(?)했다. 이후 더 이상의 삽질을 지양하기 때문에 해당 post를 남기고 필요시 참조할 생각이다.

Project의 User 를 정의함에 있어서 abstractuser 와 abstractbaseuser 를 상속하는 방법이 있다. 편하게 작업하고자 한다면 abstractuser 를 가져다 쓰는 편이 정신건강에도 좋고 시간도 절약된다.

AbstractBaseUser 는 django.contrib.auth.base_user 에 속해있다. 
> from django.contrib.auth.base_user import AbstractBaseUser

abstractuser는 abstractbaseuser를 상속받고 보편적으로 사용할 수 있게 구성되어 있다. username, first_name, last_name, email 이 기본 필드로 구성되어 있다. 물론 언급하지 않은 권한과 관련한 필드도 존재하지만 사용자에게 보여지는 필드 및 관리자가 신경써야 하는 부분이기 때문에 지나간다.

## 작성한 코드 예시

### models.py
```
class MyUserManager(BaseUserManager):
    def create_user(self, email, nickname, password):
        if not email:
            # email 를 등록 여부판단의 근거로 사용
            raise ValueError('Email 주소가 필요합니다!')

        user = self.model(
            email=email,
            nickname=nickname,
        )
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, nickname, password):
        user = self.create_user(email, nickname, password)
        user.is_admin = True
        user.is_superuser = True
        # 슈퍼 관리자 권한을 준다. 처음 이 값을 주지 않고 진행했다가 
        # admin 페이지 로그인 후 관리할 권한이 없다는 경고를 맛봤다.
        user.save(using=self._db)
        return user


class MyUser(AbstractBaseUser, PermissionsMixin):
    email = models.EmailField(max_length=40, unique=True)
    nickname = models.CharField(max_length=15, unique=True)
    username = models.CharField(max_length=10)

    is_admin = models.BooleanField(default=False)
    # 기본적으로 false 를 부여하고 superuser가 변경가능

    objects = MyUserManager()

    USERNAME_FIELD = 'email'
    # login 시에 username 의 field 에 사용할 값
    REQUIRED_FIELDS = ['nickname']
    # username_field 외에 필수 field 항목을 나타낸다.
    # 이때 username field 에 있는 값을 중복으로 넣어주면 
    # 오류가 난다.

    # createsuperuser 커맨드로 유저를 생성할 때 나타날 필드 이름 목록

    def get_nickname(self):
        return self.nickname

    def __str__(self):
        return self.email

    def get_short_name(self):
    	# 해당 method 는 admin 관리 페이지에서 사용한다.
    	# 해당 html 을 변경하지 않고 사용할 것이기 때문에
    	# 작성자가 사용하지 않아도 model에 추가한다.
        return self.nickname

    def get_full_name(self):
        return self.email

    def get_username(self):
        return self.email

    @property
    def is_staff(self):
    	# 관리권한을 주는 method 
        return self.is_admin
```

##### settings.py 에 AUTH_USER_MODEL 을 추가해준다.

```
AUTH_USER_MODEL='app_name.def_user'
```


### admin.py

```
class MyUserAdmin(UserAdmin):
    form = MyUserChangeForm
    # User 정보 변경할 때
    list_display = ('nickname', 'email', 'is_admin')
    # admin 페이지에서 관리할 항목들
    list_filter = ('is_admin',)
    fieldsets = (
        ('개인정보', {'fields': ('email', 'nickname', 'password',)}),
        ('권한', {'fields': ('is_admin',)})
    )

    add_form = MyUserCreationForm
    # User 새로 생성할 때
    add_fieldsets = (
        '기본정보', {'fields': ('email', 'nickname', 'password1', 'password2',)}
    )

    search_fields = ('email', 'nickname',)
    ordering = ('nickname',)
    # nickname 으로 정렬
    filter_horizontal = ()


admin.site.register(MyUser, MyUserAdmin)
# admin page 에 보여질 수 있게 추가

admin.site.unregister(Group)
# group 은 관리하지 않기 때문에 unregister 해준다
```

### forms.py

```
class MyUserCreationForm(UserCreationForm):
    class Meat:
        model = MyUser
        # 작성자가 위에서 만들어 놓은 MyUser 모델을 사용한다
        fields = ('email',)

        def clean_password2(self):
            password1 = self.cleaned_data.get('password1')
            password2 = self.cleaned_data.get('password2')
            if password1 and password2 and password1 != password2:
                raise forms.ValidationError("Password 일치하지 않습니다")

            return password2

        def save(self, commit=True):
        # commit 이 True 일때만 model instance save 를 호출한다. 즉, db로의 저장을 commit flag로 결정한다
            user = super(MyUserCreationForm, self).save(commit=False)
            # commit = False 일때 저장하지 않고 추후 .save() 통해 저장한다.
            user.set_password(self.cleaned_data['password1'])
            if commit:
                user.save()
            return user


class MyUserChangeForm(UserChangeForm):
    password = ReadOnlyPasswordHashField(label="비밀번호")

    class Meta:
        model = MyUser
        fields = ('email','password','is_admin')

    def clean_password(self):
        return self.initial["password"]
```


