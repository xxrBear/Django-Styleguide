# Django 风格指南

> 👀 **你的 Django 项目需要帮助吗？** [HackSoft 随时为你服务](https://www.hacksoft.io/solutions/django?utm_source=django-styleguide&utm_medium=web&utm_campaign=Django-Campaign)。联系我们：`consulting@hacksoft.io`

![Django Styleguide](logo.png)

**目录：**

<!-- toc -->

- [如何提问或提出建议？](#如何提问或提出建议)
- [这是什么？](#这是什么)
- [如何使用它？](#如何使用它)
- [概览](#概览)
- [为什么不？](#为什么不)
- [Cookie Cutter](#cookie-cutter)
- [模型（Models）](#模型models)
  - [基础模型](#基础模型)
  - [校验 - `clean` 和 `full_clean`](#校验---clean-和-full_clean)
  - [校验 - 约束（constraints）](#校验---约束constraints)
  - [属性（Properties）](#属性properties)
  - [方法（Methods）](#方法methods)
  - [测试](#测试)
- [服务（Services）](#服务services)
  - [示例 - 基于函数的服务](#示例---基于函数的服务)
  - [示例 - 基于类的服务](#示例---基于类的服务)
  - [命名约定](#命名约定)
  - [模块](#模块)
  - [选择器（Selectors）](#选择器selectors)
  - [测试](#测试-1)
- [API 与序列化器](#api-与序列化器)
  - [命名约定](#命名约定-1)
  - [基于类 vs. 基于函数](#基于类-vs-基于函数)
  - [列表 API](#列表-api)
    - [基础版](#基础版)
    - [过滤 + 分页](#过滤--分页)
  - [详情 API](#详情-api)
  - [创建 API](#创建-api)
  - [更新 API](#更新-api)
  - [获取对象](#获取对象)
  - [嵌套序列化器](#嵌套序列化器)
  - [高级序列化](#高级序列化)
- [URL 路由](#url-路由)
- [配置（Settings）](#配置settings)
  - [给环境变量加上 `DJANGO_` 前缀](#给环境变量加上-django_-前缀)
  - [第三方集成](#第三方集成)
  - [从 `.env` 读取](#从-env-读取)
- [错误与异常处理](#错误与异常处理)
  - [异常处理的工作机制（在 DRF 语境下）](#异常处理的工作机制在-drf-语境下)
    - [DRF 的 `ValidationError`](#drf-的-validationerror)
    - [Django 的 `ValidationError`](#django-的-validationerror)
  - [定义你的 API 错误长什么样](#定义你的-api-错误长什么样)
  - [掌握如何修改默认的异常处理行为](#掌握如何修改默认的异常处理行为)
  - [方案一 - 沿用 DRF 默认异常，仅做少量修改](#方案一---沿用-drf-默认异常仅做少量修改)
  - [方案二 - HackSoft 推荐的做法](#方案二---hacksoft-推荐的做法)
  - [更多思路](#更多思路)
- [测试](#测试-2)
  - [概览](#概览-1)
  - [命名约定](#命名约定-2)
  - [测试工厂（Factories）](#测试工厂factories)
- [Celery](#celery)
  - [基本原则](#基本原则)
  - [错误处理](#错误处理)
  - [配置](#配置)
  - [结构](#结构)
  - [定时任务](#定时任务)
  - [进阶](#进阶)
- [Cookbook 实战手册](#cookbook-实战手册)
  - [用服务（service）处理更新](#用服务service处理更新)
- [DX（开发者体验）](#dx开发者体验)
  - [`mypy` 与类型注解](#mypy-与类型注解)
- [Django Styleguide 的实际应用案例](#django-styleguide-的实际应用案例)
- [更多资源与替代方案](#更多资源与替代方案)
- [灵感来源](#灵感来源)

<!-- tocstop -->

## 如何提问或提出建议？

几点指引，帮你快速上手：

1. 如果你读过 Django Styleguide 之后有疑问或建议，**最简单的做法就是提一个 issue**，我们会回复
1. 即使你不确定自己的问题是否和 Django Styleguide 相关——**也尽管提 issue**，我们会回复
1. **如果你想看代码示例**，请前往 [Django Styleguide Example](https://github.com/HackSoftware/Django-Styleguide-Example) 仓库。我们把它当作一个"Django 试验项目"，既结合了最佳实践，也包含[我们博客中的示例](https://www.hacksoft.io/blog)

就这些 ✨

## 这是什么？

你好 👋

这是 Django Styleguide（Django 风格指南），由我们——[HackSoft](https://hacksoft.io) 的伙伴们创建

**关于它，有几点重要说明：**

1. 它源自多年的经验和众多大大小小的 Django 项目
1. 它是务实的。这里提到的所有做法都经过了生产环境的检验
1. 它是有明确观点的（opinionated）。这就是我们使用 Django 构建应用的方式
1. 它不是唯一的方式。还有其他构建和组织 Django 项目的方法，同样可以胜任
1. 我们有一个 [`Django-Styleguide-Example`](https://github.com/HackSoftware/Django-Styleguide-Example)，在真实项目中演示了本指南的大部分内容

**你可以观看 Radoslav Georgiev 的演讲 [Django structure for scale and longevity](https://www.youtube.com/watch?v=yG3ZdxBb1oo)，了解本风格指南背后的理念：**

[![Django structure for scale and longevity by Radoslav Georgiev](https://img.youtube.com/vi/yG3ZdxBb1oo/0.jpg)](https://www.youtube.com/watch?v=yG3ZdxBb1oo)

**你也可以观看 Radoslav Georgiev 与 Ivaylo Bachvarov 在 HackCast 上[围绕 Django Styleguide 展开的讨论](https://www.youtube.com/watch?v=9VfRaPECbpY)：**

[![HackCast S02E08 - Django Community & Django Styleguide](https://img.youtube.com/vi/9VfRaPECbpY/0.jpg)](https://www.youtube.com/watch?v=9VfRaPECbpY)

## 如何使用它？

对于 Django Styleguide，**通常有 3 种使用方式：**

1. 严格遵循这里写的所有内容
2. 结合自己的具体场景，挑选对你有意义的部分
3. 完全不遵循这里写的任何内容

**我们推荐第 2 种：**

- 通读这份风格指南
- 判断哪些内容最适合你
- 针对你的具体情况进行调整

## 概览

Django Styleguide 的核心可以归纳如下：

**在 Django 中，业务逻辑应该放在：**

- 服务（Services）——主要负责向数据库写入数据的函数
- 选择器（Selectors）——主要负责从数据库获取数据的函数
- 模型属性（Model properties）（有一些例外情况）
- 模型的 `clean` 方法，用于附加校验（有一些例外情况）

**在 Django 中，业务逻辑不应该放在：**

- API 和视图（Views）
- 序列化器（Serializers）和表单（Forms）
- 模板标签（Form tags）
- 模型的 `save` 方法
- 自定义管理器（managers）或查询集（querysets）
- 信号（Signals）

**模型属性 vs 选择器：**

- 如果一个属性需要跨越多个关联关系，那它更适合做成选择器
- 如果一个属性的逻辑不简单，并且在序列化时很容易引发 `N + 1` 查询问题，那它更适合做成选择器

总体思路是"关注点分离"（separate concerns），让这些关注点变得可维护、可测试

## 为什么不？

> 🤔 为什么不把业务逻辑放在 API / 视图 / 序列化器 / 表单里？

依赖通用 API / 视图（generic APIs / Views），再搭配上序列化器和表单，会带来两个主要问题：

1. 把业务逻辑打碎分散到多个地方，让数据流变得非常难以追踪
2. 把事情对你隐藏起来。要想改动某个东西，你必须先弄清所使用的那层抽象的内部工作原理

通用 API 和视图配合序列化器、表单，对于"对某个模型做直接 CRUD"这种简单场景确实非常好用

但根据我们迄今的经验，这种简单场景其实很少出现。**一旦你偏离了愉快的 CRUD 路径，事情就会开始变得混乱。**

而一旦事情开始混乱，你就需要更多的"盒子"，以便更好地组织代码

这份风格指南的目标是：

1. 给你这些"盒子"
1. 帮你针对自己的具体场景和需求，找到属于你自己的"盒子"

另外，多亏了这条评论——https://github.com/HackSoftware/Django-Styleguide/issues/170 ——还有一种额外的视角来看待这个问题：

你的应用应该如何运转（也就是我们所说的"业务逻辑"），不应该与你访问它的方式（无论是 API、管理命令还是别的什么）耦合在一起，而这条界线非常清晰，正是我们想要做关注点分离的地方。

当然，确实存在一些场景，两者会交织在一起。但思考这种分离时，一个好的思考基线是——"核心"（core） vs. "接口"（interface）

---

> 🤔 为什么不把业务逻辑放在自定义管理器（managers）和/或查询集（querysets）里？

这其实是个不错的主意，你完全可以引入自定义管理器和查询集，暴露出更贴合你领域模型的 API

但是，试图把所有业务逻辑都塞进自定义管理器并不是个好主意，原因如下：

1. 业务逻辑有自己的领域，它并不总是能直接映射到你的数据模型（models）上
1. 业务逻辑往往横跨多个模型，所以你很难选择该把某段逻辑放在哪里
   - 假设你有一段自定义逻辑，涉及模型 `A`、`B`、`C` 和 `D`，你该把它放在哪儿？
1. 可能还会涉及对第三方系统的额外调用。你不会希望这些调用出现在自定义管理器的方法里

**核心思路是：让你的领域逻辑独立于数据模型和 API 层存在。**

如果把"使用自定义 queryset/manager"的想法和"让领域逻辑独立存在"的想法结合起来，最终得到的产物就是我们所说的"服务层"（service layer）

**服务可以是函数、类、模块，或任何对你特定场景有意义的形式。**

话虽如此，自定义管理器和查询集是非常强大的工具，应该用来为你的模型暴露更好的接口

---

> 🤔 为什么不把业务逻辑放在信号（signals）里？

在所有可选方案中，这一个也许会最快把你带进沟里：

1. 信号是一个优秀的工具，用于**连接那些本不应该互相知道对方存在、但你又希望它们关联起来的东西**
1. 信号同样是一个优秀的工具，用于**在业务逻辑层之外处理缓存失效**
1. 如果我们开始用信号处理那些耦合很重的逻辑，我们只是在让这种关联变得更隐晦，让数据流更难追踪

这就是为什么我们推荐只在非常特定的场景下使用信号。总的来说，**我们不推荐用信号来组织领域层 / 业务层**

## Cookie Cutter

我们建议每个新项目都从某种 cookiecutter 模板开始。从一开始就拥有正确的项目结构，会带来回报

几个例子：

- 你可以把 [`Styleguide-Example`](https://github.com/HackSoftware/Styleguide-Example) 项目作为起点
- 你也可以使用 [`cookiecutter-django`](https://github.com/pydanny/cookiecutter-django)，里面有一大堆好东西
- 或者你可以打造适合自己场景的模板，把它变成一个 [cookiecutter](https://cookiecutter.readthedocs.io/en/latest/) 项目

## 模型（Models）

模型应该只负责数据模型本身，不应承担太多其他职责

### 基础模型

定义一个可供继承的 `BaseModel` 是个好主意

通常，像 `created_at` 和 `updated_at` 这样的字段，是放进 `BaseModel` 的完美候选

下面是一个 `BaseModel` 示例：

```python
from django.db import models
from django.utils import timezone


class BaseModel(models.Model):
    created_at = models.DateTimeField(db_index=True, default=timezone.now)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True
```

之后，每当你需要一个新模型，直接继承 `BaseModel` 即可：

```python
class SomeModel(BaseModel):
    pass
```

### 校验 - `clean` 和 `full_clean`

我们来看一个示例模型：

```python
class Course(BaseModel):
    name = models.CharField(unique=True, max_length=255)

    start_date = models.DateField()
    end_date = models.DateField()

    def clean(self):
        if self.start_date >= self.end_date:
            raise ValidationError("End date cannot be before start date")
```

我们定义模型的 `clean` 方法，是为了确保进入数据库的数据是合格的

不过，要让 `clean` 方法被调用，就必须有人在保存之前对模型实例调用 `full_clean`

**我们的建议是：在服务（service）中、调用 save 之前执行这一步：**

```python
def course_create(*, name: str, start_date: date, end_date: date) -> Course:
    obj = Course(name=name, start_date=start_date, end_date=end_date)

    obj.full_clean()
    obj.save()

    return obj
```

这种做法也能和 Django admin 很好地配合，因为 admin 中使用的表单会触发实例的 `full_clean`

**关于什么时候把校验加进模型的 `clean` 方法，我们有几条大致的经验法则：**

1. 校验基于模型的多个**非关联字段**
1. 校验逻辑本身足够简单

**出现以下情况时，校验应该移到服务层：**

1. 校验逻辑更复杂
1. 需要跨越关联关系、获取额外的数据

> 在 `clean` 和服务中同时存在校验是没问题的，但在这种情况下，我们倾向于把逻辑挪到服务里

### 校验 - 约束（constraints）

正如[这个 issue](https://github.com/HackSoftware/Django-Styleguide/issues/22) 中提出的，如果可以使用 [Django 的约束（constraints）](https://docs.djangoproject.com/en/dev/ref/models/constraints/)来做校验，那就应该优先用约束

要写的代码更少，要维护的代码更少，而且即使数据从别的地方写入，数据库也会替你把好关

来看个例子！

```python
class Course(BaseModel):
    name = models.CharField(unique=True, max_length=255)

    start_date = models.DateField()
    end_date = models.DateField()

    class Meta:
        constraints = [
            models.CheckConstraint(
                name="start_date_before_end_date",
                check=Q(start_date__lt=F("end_date"))
            )
        ]
```

现在，如果我们尝试通过 `course.save()` 或 `Course.objects.create(...)` 创建新对象，我们会得到 `IntegrityError`，而不是 `ValidationError`

这实际上是这种做法的一个缺点（_从 Django 4.1 开始情况不同了，见下面的补充说明_），因为现在我们必须处理 `IntegrityError`，而它的错误信息并不总是那么友好

> 👀 ⚠️ 👀 从 Django 4.1 开始，调用 `.full_clean` 也会检查模型约束了！
>
> 这实际上消除了上面提到的缺点，因为如果模型约束校验失败，你会得到一个友好的 `ValidationError`（如果你走 `Model.objects.create(...)` 这条路，缺点依然存在）
>
> 更多内容见这里 - <https://docs.djangoproject.com/en/4.1/ref/models/instances/#validating-objects>
>
> 示例测试用例见 Styleguide-Example 仓库 - <https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/common/tests/models/test_random_model.py#L12>

Django 官方文档中关于约束的部分相当简略，所以你可以参考 Adam Johnson 的以下文章，了解如何使用它们：

1. [Using Django Check Constraints to Ensure Only One Field Is Set](https://adamj.eu/tech/2020/03/25/django-check-constraints-one-field-set/)
1. [Django’s Field Choices Don’t Constrain Your Data](https://adamj.eu/tech/2020/01/22/djangos-field-choices-dont-constrain-your-data/)
1. [Using Django Check Constraints to Prevent Self-Following](https://adamj.eu/tech/2021/02/26/django-check-constraints-prevent-self-following/)

### 属性（Properties）

模型属性是快速访问模型实例派生值的好方法

举个例子，看看我们 `Course` 模型的 `has_started` 和 `has_finished` 属性：

```python
from django.utils import timezone
from django.core.exceptions import ValidationError


class Course(BaseModel):
    name = models.CharField(unique=True, max_length=255)

    start_date = models.DateField()
    end_date = models.DateField()

    def clean(self):
        if self.start_date >= self.end_date:
            raise ValidationError("End date cannot be before start date")

    @property
    def has_started(self) -> bool:
        now = timezone.now()

        return self.start_date <= now.date()

    @property
    def has_finished(self) -> bool:
        now = timezone.now()

        return self.end_date <= now.date()
```

这些属性很好用，因为我们现在可以在序列化器中引用它们，或者在模板中使用它们

**关于什么时候给模型添加属性，我们有几条大致的经验法则：**

1. 如果需要一个基于**非关联模型字段**的简单派生值，就为它添加一个 `@property`
1. 如果派生值的计算足够简单

**以下情况中，这些逻辑应该放到别处（服务、选择器或工具函数）：**

1. 需要跨越多个关联关系或获取额外数据
1. 计算逻辑更复杂

请记住，这些规则是模糊的，因为上下文往往很重要。运用你的最佳判断！

### 方法（Methods）

模型方法同样是非常强大的工具，可以构建在属性之上

我们来看一个 `is_within(self, x)` 方法的例子：

```python
from django.core.exceptions import ValidationError
from django.utils import timezone


class Course(BaseModel):
    name = models.CharField(unique=True, max_length=255)

    start_date = models.DateField()
    end_date = models.DateField()

    def clean(self):
        if self.start_date >= self.end_date:
            raise ValidationError("End date cannot be before start date")

    @property
    def has_started(self) -> bool:
        now = timezone.now()

        return self.start_date <= now.date()

    @property
    def has_finished(self) -> bool:
        now = timezone.now()

        return self.end_date <= now.date()

    def is_within(self, x: date) -> bool:
        return self.start_date <= x <= self.end_date
```

`is_within` 无法做成属性，因为它需要接收参数，所以它是个方法

在模型中使用方法还有一种绝佳场景——**属性赋值**：当设置某个属性时必须总是连带设置另一个由它派生的属性

举个例子：

```python
from django.utils.crypto import get_random_string
from django.conf import settings
from django.utils import timezone


class Token(BaseModel):
    secret = models.CharField(max_length=255, unique=True)
    expiry = models.DateTimeField(blank=True, null=True)

    def set_new_secret(self):
        now = timezone.now()

        self.secret = get_random_string(255)
        self.expiry = now + settings.TOKEN_EXPIRY_TIMEDELTA

        return self
```

现在，我们可以安全地调用 `set_new_secret`，它会给 `secret` 和 `expiry` 都生成正确的值

**关于什么时候给模型添加方法，我们有几条大致的经验法则：**

1. 如果需要一个基于**非关联模型字段**、且需要接收参数的简单派生值，就为它添加一个方法
1. 如果派生值的计算足够简单
1. 如果设置一个属性总是需要连带设置其他属性的值，就用方法来做

**以下情况中，这些逻辑应该放到别处（服务、选择器或工具函数）：**

1. 需要跨越多个关联关系或获取额外数据
1. 计算逻辑更复杂

请记住，这些规则是模糊的，因为上下文往往很重要。运用你的最佳判断！

### 测试

模型只有在包含额外逻辑时才需要测试——比如校验、属性或方法

举个例子：

```python
from datetime import timedelta

from django.test import TestCase
from django.core.exceptions import ValidationError
from django.utils import timezone

from project.some_app.models import Course


class CourseTests(TestCase):
    def test_course_end_date_cannot_be_before_start_date(self):
        start_date = timezone.now()
        end_date = timezone.now() - timedelta(days=1)

        course = Course(start_date=start_date, end_date=end_date)

        with self.assertRaises(ValidationError):
            course.full_clean()
```

这里有几点需要注意：

1. 我们断言：调用 `full_clean` 时会抛出校验错误
1. **我们完全没有触碰数据库**，因为没有必要。这能加快某些测试的速度

## 服务（Services）

服务是业务逻辑存放的地方

服务层使用软件特定的领域语言，可以访问数据库及其他资源，并能与系统的其他部分交互

下面是一张非常简单的示意图，展示了服务层在我们 Django 应用中的位置：

![Service layer](https://user-images.githubusercontent.com/387867/134778130-be168592-b953-4b74-8588-a3dbaa0b6871.png)

一个服务可以是：

- 一个简单的函数
- 一个类
- 一整个模块
- 任何对你场景有意义的形式

在大多数情况下，服务可以是一个简单的函数，它：

- 位于 `<your_app>/services.py` 模块中
- 只接收关键字参数（keyword-only arguments），除非它不需要参数或只需要一个参数
- 带有类型注解（即使你目前还没用上 [`mypy`](https://github.com/python/mypy)）
- 与数据库、其他资源以及系统的其他部分交互
- 承载业务逻辑——从简单的模型创建，到复杂的横切关注点（cross-cutting concerns），再到调用外部服务和任务

### 示例 - 基于函数的服务

一个创建用户的示例服务：

```python
def user_create(
    *,
    email: str,
    name: str
) -> User:
    user = User(email=email)
    user.full_clean()
    user.save()

    profile_create(user=user, name=name)
    confirmation_email_send(user=user)

    return user
```

如你所见，这个服务调用了另外两个服务——`profile_create` 和 `confirmation_email_send`。

在这个例子中，与用户创建相关的所有逻辑都集中在一个地方，可以被追踪。

### 示例 - 基于类的服务

**此外，我们还可以有"基于类的"服务**，用花哨点的话说就是——把逻辑包裹在一个类里。

下面是一个直接取自 [Django Styleguide Example](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/services.py#L22) 的示例，与文件上传相关：

```python
# https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/services.py


class FileStandardUploadService:
    """
    This also serves as an example of a service class,
    which encapsulates 2 different behaviors (create & update) under a namespace.

    Meaning, we use the class here for:

    1. The namespace
    2. The ability to reuse `_infer_file_name_and_type` (which can also be an util)
    """
    def __init__(self, user: BaseUser, file_obj):
        self.user = user
        self.file_obj = file_obj

    def _infer_file_name_and_type(self, file_name: str = "", file_type: str = "") -> Tuple[str, str]:
        file_name = file_name or self.file_obj.name

        if not file_type:
            guessed_file_type, encoding = mimetypes.guess_type(file_name)
            file_type = guessed_file_type or ""

        return file_name, file_type

    @transaction.atomic
    def create(self, file_name: str = "", file_type: str = "") -> File:
        _validate_file_size(self.file_obj)

        file_name, file_type = self._infer_file_name_and_type(file_name, file_type)

        obj = File(
            file=self.file_obj,
            original_file_name=file_name,
            file_name=file_generate_name(file_name),
            file_type=file_type,
            uploaded_by=self.user,
            upload_finished_at=timezone.now()
        )

        obj.full_clean()
        obj.save()

        return obj

    @transaction.atomic
    def update(self, file: File, file_name: str = "", file_type: str = "") -> File:
        _validate_file_size(self.file_obj)

        file_name, file_type = self._infer_file_name_and_type(file_name, file_type)

        file.file = self.file_obj
        file.original_file_name = file_name
        file.file_name = file_generate_name(file_name)
        file.file_type = file_type
        file.uploaded_by = self.user
        file.upload_finished_at = timezone.now()

        file.full_clean()
        file.save()

        return file
```

正如注释中所说，我们采用这种做法主要出于两个原因：

1. **命名空间。** 我们的 create 和 update 共处同一个命名空间。
1. **复用** `_infer_file_name_and_type` 这段逻辑。

下面是这个服务的用法：

```python
# https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/apis.py

class FileDirectUploadApi(ApiAuthMixin, APIView):
    def post(self, request):
        service = FileDirectUploadService(
            user=request.user,
            file_obj=request.FILES["file"]
        )
        file = service.create()

        return Response(data={"id": file.id}, status=status.HTTP_201_CREATED)
```

以及

```python
@admin.register(File)
class FileAdmin(admin.ModelAdmin):
    # ... other code here ...
    # https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/admin.py

    def save_model(self, request, obj, form, change):
        try:
            cleaned_data = form.cleaned_data

            service = FileDirectUploadService(
                file_obj=cleaned_data["file"],
                user=cleaned_data["uploaded_by"]
            )

            if change:
                service.update(file=obj)
            else:
                service.create()
        except ValidationError as exc:
            self.message_user(request, str(exc), messages.ERROR)
```

另外，基于类的服务也很适合"流程"（flows）——需要经过多个步骤的事情。

例如，下面这个服务代表一个"文件直传流程"，包含 `start` 和 `finish`（以及额外步骤）：

```python
# https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/services.py


class FileDirectUploadService:
    """
    This also serves as an example of a service class,
    which encapsulates a flow (start & finish) + one-off action (upload_local) into a namespace.

    Meaning, we use the class here for:

    1. The namespace
    """
    def __init__(self, user: BaseUser):
        self.user = user

    @transaction.atomic
    def start(self, *, file_name: str, file_type: str) -> Dict[str, Any]:
        file = File(
            original_file_name=file_name,
            file_name=file_generate_name(file_name),
            file_type=file_type,
            uploaded_by=self.user,
            file=None
        )
        file.full_clean()
        file.save()

        upload_path = file_generate_upload_path(file, file.file_name)

        """
        We are doing this in order to have an associated file for the field.
        """
        file.file = file.file.field.attr_class(file, file.file.field, upload_path)
        file.save()

        presigned_data: Dict[str, Any] = {}

        if settings.FILE_UPLOAD_STORAGE == FileUploadStorage.S3:
            presigned_data = s3_generate_presigned_post(
                file_path=upload_path, file_type=file.file_type
            )

        else:
            presigned_data = {
                "url": file_generate_local_upload_url(file_id=str(file.id)),
            }

        return {"id": file.id, **presigned_data}

    @transaction.atomic
    def finish(self, *, file: File) -> File:
        # Potentially, check against user
        file.upload_finished_at = timezone.now()
        file.full_clean()
        file.save()

        return file
```

### 命名约定

命名约定取决于你的口味。在整个项目中保持一致的约定会带来回报。

以上面的例子来说，我们的服务命名为 `user_create`。其模式是——`<实体>_<动作>`。

这是我们在 HackSoft 项目中偏好的方式。乍一看有点怪，但它有几个不错的特性：

- **命名空间化。** 很容易一眼找出所有以 `user_` 开头的服务，而且把它们放进一个 `users.py` 模块里是个好主意。
- **可搜索性（Greppability）。** 换句话说，如果你想查看某个实体的所有操作，只需全局搜索 `user_`。

### 模块

如果你的 Django 应用足够简单、只有一堆服务，它们可以愉快地全部住在 `service.py` 模块里。

但当规模变大时，你可能想把 `services.py` 拆分成一个包含子模块的文件夹，按照应用中不同的子领域来划分。

举个例子，假设我们有一个 `authentication` 应用，在 `services` 模块中有 1 个处理 `jwt` 的子模块，以及一个处理 `oauth` 的子模块。

结构可能长这样：

```
services
├── __init__.py
├── jwt.py
└── oauth.py
```

这里有很多玩法：

- 你可以在 `services/__init__.py` 中做导入-导出（import-export dance），这样其他地方就能直接从 `project.authentication.services` 导入。
- 你可以创建一个文件夹模块 `jwt/__init__.py`，把代码放在里面。
- 基本上，结构由你决定。如果你觉得是时候重构、调整结构了——那就动手。

### 选择器（Selectors）

在我们的大多数项目中，我们会区分"向数据库推送数据"和"从数据库拉取数据"：

1. 服务负责推送。
1. **选择器负责拉取。**
1. 选择器可以被视为服务的一个"子层"，专门负责获取数据。

> 如果这个理念不能引起你的共鸣，你完全可以只用服务来承担这两类操作。

选择器遵循与服务相同的规则。

例如，在 `<your_app>/selectors.py` 模块中，我们可以这样写：

```python
def user_list(*, fetched_by: User) -> Iterable[User]:
    user_ids = user_get_visible_for(user=fetched_by)

    query = Q(id__in=user_ids)

    return User.objects.filter(query)
```

如你所见，`user_get_visible_for` 是另一个选择器。

你可以返回查询集、列表，或任何对你具体场景有意义的东西。

### 测试

由于服务承载着我们的业务逻辑，它们是测试的理想对象。

如果你决定用测试覆盖服务层，我们有几条大致的经验法则：

1. 测试**应该穷尽地覆盖业务逻辑**。
1. 测试**应该访问数据库**——既创建也读取。
1. 测试**应该 mock 掉异步任务调用以及一切跳出本项目的外部调用**。

在为某个测试创建所需的状态时，可以组合使用：

- 假数据（Fakes，我们推荐使用 [`faker`](https://github.com/joke2k/faker)）
- 其他服务，用来创建所需的对象。
- 专门的测试工具与辅助方法。
- 工厂（Factories，我们推荐使用 [`factory_boy`](https://factoryboy.readthedocs.io/en/latest/orms.html)）
- 如果项目还没引入工厂，就直接用 `Model.objects.create()` 调用。
- 总之，什么顺手用什么。

**我们来看看示例中的这个服务：**

```python
from django.contrib.auth.models import User
from django.core.exceptions import ValidationError
from django.db import transaction

from project.payments.selectors import items_get_for_user
from project.payments.models import Item, Payment
from project.payments.tasks import payment_charge


@transaction.atomic
def item_buy(
    *,
    item: Item,
    user: User,
) -> Payment:
    if item in items_get_for_user(user=user):
        raise ValidationError(f'Item {item} already in {user} items.')

    payment = Payment(
        item=item,
        user=user,
        successful=False
    )
    payment.full_clean()
    payment.save()

    # Run the task once the transaction has commited,
    # guaranteeing the object has been created.
    transaction.on_commit(
        lambda: payment_charge.delay(payment_id=payment.id)
    )

    return payment
```

这个服务：

- 调用一个选择器做校验。
- 创建一个对象。
- 延迟执行一个任务。

**这是我们的测试：**

```python
from unittest.mock import patch, Mock

from django.test import TestCase
from django.contrib.auth.models import User
from django.core.exceptions import ValidationError

from django_styleguide.payments.services import item_buy
from django_styleguide.payments.models import Payment, Item


class ItemBuyTests(TestCase):
    @patch('project.payments.services.items_get_for_user')
    def test_buying_item_that_is_already_bought_fails(
        self, items_get_for_user_mock: Mock
    ):
        """
        Since we already have tests for `items_get_for_user`,
        we can safely mock it here and give it a proper return value.
        """
        user = User(username='Test User')
        item = Item(
            name='Test Item',
            description='Test Item description',
            price=10.15
        )

        items_get_for_user_mock.return_value = [item]

        with self.assertRaises(ValidationError):
            item_buy(user=user, item=item)

    @patch('project.payments.services.payment_charge.delay')
    def test_buying_item_creates_a_payment_and_calls_charge_task(
        self,
        payment_charge_mock: Mock
    ):
        # How we prepare our tests is a topic for a different discussion
        user = given_a_user(username="Test user")
        item = given_a_item(
            name='Test Item',
            description='Test Item description',
            price=10.15
        )

        self.assertEqual(0, Payment.objects.count())

        payment = item_buy(user=user, item=item)

        self.assertEqual(1, Payment.objects.count())
        self.assertEqual(payment, Payment.objects.first())

        self.assertFalse(payment.successful)

        payment_charge_mock.assert_called_once()
```

## API 与序列化器

当使用服务与选择器时，你所有的 API 看起来都应该简单而一致。

**在创建新 API 时，我们遵循以下通用规则：**

- 一个操作对应一个 API。也就是说，对一个模型的 CRUD，要有 4 个 API。
- 继承自最简单的 `APIView` 或 `GenericAPIView`。
  - 避免使用更抽象的类，因为它们倾向于通过序列化器来管理逻辑，而我们希望通过服务和选择器来做。
- **不要在 API 中写业务逻辑。**
- 你可以**在 API 中做对象获取 / 数据加工**（如果有必要，也可以把这部分抽离到别处）。
  - 如果你在 API 中调用了 `some_service`，你可以把对象获取 / 数据加工抽离到 `some_service_parse` 中。
- 总而言之，让 API 尽可能简单。它们是通往你核心业务逻辑的接口。

谈到 API，我们需要一种处理数据序列化的方式——包括进来的数据和出去的数据。

**这是我们关于 API 序列化的规则：**

- 应该有专门的**输入序列化器**和专门的**输出序列化器**。
- **输入序列化器**负责处理进来的数据。
- **输出序列化器**负责处理出去的数据。
- 在序列化层面，使用任何适合你的抽象都可以。

**如果你使用 DRF 的序列化器，这是我们的规则：**

- 序列化器应该**嵌套在 API 类内部**，并命名为 `InputSerializer` 或 `OutputSerializer`。
- 我们更偏好让两个序列化器都继承自更简单的 `Serializer`，避免使用 `ModelSerializer`。
  - 这是偏好和选择的问题。如果 `ModelSerializer` 对你很顺手，那就用它。
- 如果需要嵌套序列化器，使用 `inline_serializer` 工具。
- 尽可能少地复用序列化器。
  - 复用序列化器可能会让你遭遇意外行为——当基础序列化器发生变化时。

### 命名约定

我们的 API 使用如下命名约定：`<实体><动作>Api`。

举几个例子：`UserCreateApi`、`UserSendResetPasswordApi`、`UserDeactivateApi` 等等。

### 基于类 vs. 基于函数

> 这主要取决于个人偏好，因为两种方式都能达成同样的结果。

我们的偏好如下：

1. 默认选择基于类的 API / 视图。
1. 如果团队里其他人都更偏好、也更习惯函数，就用基于函数的 API / 视图。

对我们来说，使用类来写 API / 视图带来的额外好处是：

1. 你可以继承一个 `BaseApi`，或者添加 mixin。
   - 如果使用基于函数的 API / 视图，你也得做同样的事，只是要用装饰器。
2. 类创建了一个命名空间，你可以在里面嵌套东西（属性、方法等）。
   - 额外的 API 配置可以通过类属性完成。
   - 而在基于函数的 API / 视图中，你需要堆叠装饰器。

下面是一个继承 `BaseApi` 的类示例：

```python
class SomeApi(BaseApi):
    def get(self, request):
        data = something()

        return Response(data)
```

下面是一个使用 `base_api` 装饰器的函数示例（具体实现取决于你的需求）：

```python
@base_api(["GET"])
def some_api(request):
    data = something()
    return Response(data)
```

### 列表 API

#### 基础版

一个极简的列表 API 应该长这样：

```python
from rest_framework.views import APIView
from rest_framework import serializers
from rest_framework.response import Response

from styleguide_example.users.selectors import user_list
from styleguide_example.users.models import BaseUser


class UserListApi(APIView):
    class OutputSerializer(serializers.Serializer):
        id = serializers.CharField()
        email = serializers.CharField()

    def get(self, request):
        users = user_list()

        data = self.OutputSerializer(users, many=True).data

        return Response(data)
```

_请注意，这个 API 默认是公开的。如何做认证由你决定。_

#### 过滤 + 分页

乍一看这有点棘手，因为我们的 API 继承的是 DRF 最基础的 `APIView`，而过滤和分页功能是内置在通用视图（generic views）里的：

1. [DRF Filtering](https://www.django-rest-framework.org/api-guide/filtering/)
1. [DRF Pagination](https://www.django-rest-framework.org/api-guide/pagination/)

因此我们采用以下做法：

1. 选择器负责实际的过滤。
1. API 负责过滤参数的序列化。
1. 如果你需要 DRF 提供的某种通用分页，API 应该负责处理。
1. 如果你需要不同的分页方式，或者自己实现分页，要么加一层专门处理分页，要么让选择器替你完成。

**我们来看一个依赖 DRF 自带分页的例子：**

```python
from rest_framework.views import APIView
from rest_framework import serializers

from styleguide_example.api.mixins import ApiErrorsMixin
from styleguide_example.api.pagination import get_paginated_response, LimitOffsetPagination

from styleguide_example.users.selectors import user_list
from styleguide_example.users.models import BaseUser


class UserListApi(ApiErrorsMixin, APIView):
    class Pagination(LimitOffsetPagination):
        default_limit = 1

    class FilterSerializer(serializers.Serializer):
        id = serializers.IntegerField(required=False)
        # Important: If we use BooleanField, it will default to False
        is_admin = serializers.NullBooleanField(required=False)
        email = serializers.EmailField(required=False)

    class OutputSerializer(serializers.Serializer):
        id = serializers.CharField()
        email = serializers.CharField()
        is_admin = serializers.BooleanField()

    def get(self, request):
        # Make sure the filters are valid, if passed
        filters_serializer = self.FilterSerializer(data=request.query_params)
        filters_serializer.is_valid(raise_exception=True)

        users = user_list(filters=filters_serializer.validated_data)

        return get_paginated_response(
            pagination_class=self.Pagination,
            serializer_class=self.OutputSerializer,
            queryset=users,
            request=request,
            view=self
        )
```

观察这个 API，我们可以发现几点：

1. 有一个 `FilterSerializer`，负责处理查询参数。如果不在这里做，就得在别的地方做，而 DRF 的序列化器非常擅长这件事。
1. 我们把过滤器传给了 `user_list` 选择器。
1. 我们使用 `get_paginated_response` 工具函数来返回一个……分页响应。

接下来看看选择器：

```python
import django_filters

from styleguide_example.users.models import BaseUser


class BaseUserFilter(django_filters.FilterSet):
    class Meta:
        model = BaseUser
        fields = ('id', 'email', 'is_admin')


def user_list(*, filters=None):
    filters = filters or {}

    qs = BaseUser.objects.all()

    return BaseUserFilter(filters, qs).qs
```

如你所见，我们利用了强大的 [`django-filter`](https://django-filter.readthedocs.io/en/stable/) 库。

> 👀 这里的关键点是：过滤由选择器负责。你当然可以用别的东西作为过滤抽象。在大多数情况下，`django-filter` 已经绰绰有余。

最后看看 `get_paginated_response`：

```python
from rest_framework.response import Response


def get_paginated_response(*, pagination_class, serializer_class, queryset, request, view):
    paginator = pagination_class()

    page = paginator.paginate_queryset(queryset, request, view=view)

    if page is not None:
        serializer = serializer_class(page, many=True)
        return paginator.get_paginated_response(serializer.data)

    serializer = serializer_class(queryset, many=True)

    return Response(data=serializer.data)
```

这基本上是从 DRF 内部提取出来的代码。

`LimitOffsetPagination` 也一样：

```python
from collections import OrderedDict

from rest_framework.pagination import LimitOffsetPagination as _LimitOffsetPagination
from rest_framework.response import Response


class LimitOffsetPagination(_LimitOffsetPagination):
    default_limit = 10
    max_limit = 50

    def get_paginated_data(self, data):
        return OrderedDict([
            ('limit', self.limit),
            ('offset', self.offset),
            ('count', self.count),
            ('next', self.get_next_link()),
            ('previous', self.get_previous_link()),
            ('results', data)
        ])

    def get_paginated_response(self, data):
        """
        We redefine this method in order to return `limit` and `offset`.
        This is used by the frontend to construct the pagination itself.
        """
        return Response(OrderedDict([
            ('limit', self.limit),
            ('offset', self.offset),
            ('count', self.count),
            ('next', self.get_next_link()),
            ('previous', self.get_previous_link()),
            ('results', data)
        ]))
```

我们所做的，本质上是把通用 API 的逻辑逆向拆解了出来。

> 👀 再说一次，如果你需要别的分页方式，完全可以自己实现，然后以同样的方式使用。有些场景下，分页需要由选择器负责。我们处理这些场景的方式与处理过滤相同。

你可以在 [Styleguide Example](https://github.com/HackSoftware/Styleguide-Example#example-list-api) 项目中找到这个带过滤和分页的列表示例 API 的代码。

### 详情 API

举个例子：

```python
class CourseDetailApi(SomeAuthenticationMixin, APIView):
    class OutputSerializer(serializers.Serializer):
        id = serializers.CharField()
        name = serializers.CharField()
        start_date = serializers.DateField()
        end_date = serializers.DateField()

    def get(self, request, course_id):
        course = course_get(id=course_id)

        serializer = self.OutputSerializer(course)

        return Response(serializer.data)
```

### 创建 API

举个例子：

```python
class CourseCreateApi(SomeAuthenticationMixin, APIView):
    class InputSerializer(serializers.Serializer):
        name = serializers.CharField()
        start_date = serializers.DateField()
        end_date = serializers.DateField()

    def post(self, request):
        serializer = self.InputSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)

        course_create(**serializer.validated_data)

        return Response(status=status.HTTP_201_CREATED)
```

### 更新 API

举个例子：

```python
class CourseUpdateApi(SomeAuthenticationMixin, APIView):
    class InputSerializer(serializers.Serializer):
        name = serializers.CharField(required=False)
        start_date = serializers.DateField(required=False)
        end_date = serializers.DateField(required=False)

    def post(self, request, course_id):
        serializer = self.InputSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)

        course_update(course_id=course_id, **serializer.validated_data)

        return Response(status=status.HTTP_200_OK)
```

### 获取对象

当我们的 API 收到一个 `object_id` 时，问题来了：**应该在哪里获取这个对象？**

我们有几个选项：

1. 可以把 id 传给序列化器，使用 [`PrimaryKeyRelatedField`](https://www.django-rest-framework.org/api-guide/relations/#primarykeyrelatedfield)（或者用 [`SlugRelatedField`](https://www.django-rest-framework.org/api-guide/relations/#slugrelatedfield) 也行）。
1. 可以在 API 层做对象获取，然后把对象传给服务或选择器。
1. 可以把 id 传给服务 / 选择器，在那里做对象获取。

采用哪种方式，取决于项目上下文和偏好。

我们通常的做法是在 API 层获取对象，使用一个专门的 `get_object` 工具函数：

```python
def get_object(model_or_queryset, **kwargs):
    """
    Reuse get_object_or_404 since the implementation supports both Model && queryset.
    Catch Http404 & return None
    """
    try:
        return get_object_or_404(model_or_queryset, **kwargs)
    except Http404:
        return None
```

这是一个非常基础的工具函数，它捕获异常并返回 `None`。

无论你选择哪种方式，确保保持一致。

### 嵌套序列化器

如果你需要使用嵌套序列化器，可以这样做：

```python
class Serializer(serializers.Serializer):
    weeks = inline_serializer(many=True, fields={
        'id': serializers.IntegerField(),
        'number': serializers.IntegerField(),
    })
```

`inline_serializer` 的实现可以在 [Styleguide-Example](https://github.com/HackSoftware/Styleguide-Example) 仓库中[这里](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/api/utils.py)找到。

### 高级序列化

有时候，一个 API 的最终结果会相当复杂。有时候，我们想优化查询，而优化本身也可能相当复杂。

在这种情况下，试图只用一个 `OutputSerializer` 可能会限制我们的选择。

这时，我们可以把输出序列化实现为一个函数，把需要的优化放在那里，**而不是把所有优化都塞进选择器里。**

以下面这个 API 为例：

```python
class SomeGenericFeedApi(BaseApi):
    def get(self, request):
        feed = some_feed_get(
            user=request.user,
        )

        data = some_feed_serialize(feed)

        return Response(data)
```

在这个场景中，`some_feed_get` 的职责是返回一个信息流条目列表（可以是 ORM 对象，可以只是 ID，也可以是任何适合你的东西）。

而我们希望把"以最优方式序列化这个信息流"的复杂性，推到序列化函数 `some_feed_serialize` 里。

这意味着我们不必在 `some_feed_get` 中做任何不必要的预取（prefetch）和优化。

下面是一个 `some_feed_serialize` 的示例：

```python
class FeedItemSerializer(serializers.Serializer):
    ... some fields here ...
    calculated_field = serializers.IntegerField(source="_calculated_field")


def some_feed_serialize(feed: List[FeedItem]):
    feed_ids = [feed_item.id for feed_item in feed]

    # Refetch items with more optimizations
    # Based on the relations that are going in
    objects = FeedItem.objects.select_related(
      # ... as complex as you want ...
    ).prefetch_related(
      # ... as complex as you want ...
    ).filter(
      id__in=feed_ids
    ).order_by(
      "-some_timestamp"
    )

    some_cache = get_some_cache(feed_ids)

    result = []

    for feed_item in objects:
        # An example, adding additional fields for the serializer
        # That are based on values outside of our current object
        # This may be some optimization to save queries
        feed_item._calculated_field = some_cache.get(feed_item.id)

        result.append(FeedItemSerializer(feed_item).data)

    return result
```

如你所见，这是一个相当通用的例子，但理念很简单：

1. 重新获取数据，带上所需的 join 和 prefetch。
1. 获取或构建内存缓存，为特定的计算值节省查询。
1. 返回一个可以直接作为 API 响应的结果。

尽管这被标记为"高级序列化"，但这个模式非常强大，可以用于所有序列化场景。

这类序列化函数通常住在对应 Django 应用的 `serializers.py` 模块中。

## URL 路由

我们通常用组织 API 的同样方式来组织 URL——一个 API 对应一个 URL，也就是一个动作对应一个 URL。

一条大致的经验法则是：把不同领域的 URL 拆到各自的 `domain_patterns` 列表中，然后在 `urlpatterns` 里 include 进来。

下面是用上面那些 API 做的例子：

```python
from django.urls import path, include

from project.education.apis import (
    CourseCreateApi,
    CourseUpdateApi,
    CourseListApi,
    CourseDetailApi,
    CourseSpecificActionApi,
)


course_patterns = [
    path('', CourseListApi.as_view(), name='list'),
    path('<int:course_id>/', CourseDetailApi.as_view(), name='detail'),
    path('create/', CourseCreateApi.as_view(), name='create'),
    path('<int:course_id>/update/', CourseUpdateApi.as_view(), name='update'),
    path(
        '<int:course_id>/specific-action/',
        CourseSpecificActionApi.as_view(),
        name='specific-action'
    ),
]

urlpatterns = [
    path('courses/', include((course_patterns, 'courses'))),
]
```

**这样拆分 URL 能给你额外的灵活性，可以把不同领域的 URL 模式移到独立的模块中**，这对于非常大的项目尤其有用——在那种项目里，你会经常在 `urls.py` 上遇到合并冲突。

另外，如果你想看到完整的 URL 树状结构，也完全可以做到——只要不把 include 的 URL 提取成单独的变量即可。

下面是来自我们 [Django Styleguide Example](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/urls.py) 的例子：

```python
from django.urls import path, include

from styleguide_example.files.apis import (
    FileDirectUploadApi,

    FilePassThruUploadStartApi,
    FilePassThruUploadFinishApi,
    FilePassThruUploadLocalApi,
)


urlpatterns = [
    path(
        "upload/",
        include(([
            path(
                "direct/",
                FileDirectUploadApi.as_view(),
                name="direct"
            ),
            path(
                "pass-thru/",
                include(([
                    path(
                        "start/",
                        FilePassThruUploadStartApi.as_view(),
                        name="start"
                    ),
                    path(
                        "finish/",
                        FilePassThruUploadFinishApi.as_view(),
                        name="finish"
                    ),
                    path(
                        "local/<str:file_id>/",
                        FilePassThruUploadLocalApi.as_view(),
                        name="local"
                    )
                ], "pass-thru"))
            )
        ], "upload"))
    )
]
```

有人偏好第一种写法，也有人偏好可见的树状结构。这取决于你和你的团队。

## 配置（Settings）

关于 Django 配置，我们倾向于遵循 [`cookiecutter-django`](https://github.com/cookiecutter/cookiecutter-django) 的目录结构，并做了一些调整：

- 我们把 Django 专属配置与其他配置分开。
- 所有东西都应该被包含进 `base.py`。
  - 不应该存在只在 `production.py` 中包含的东西。
  - 只在生产环境生效的东西，通过环境变量来控制。

下面是我们 [`Styleguide-Example`](https://github.com/HackSoftware/Styleguide-Example) 项目的目录结构：

```
config
├── __init__.py
├── django
│   ├── __init__.py
│   ├── base.py
│   ├── local.py
│   ├── production.py
│   └── test.py
├── settings
│   ├── __init__.py
│   ├── celery.py
│   ├── cors.py
│   ├── sentry.py
│   └── sessions.py
├── urls.py
├── env.py
└── wsgi.py
├── asgi.py
```

在 `config/django` 中，我们放所有与 Django 相关的配置：

- `base.py` 包含大部分配置，并从 `config/settings` 导入所有其他配置。
- `production.py` 从 `base.py` 导入，然后覆盖一些生产环境特定的配置。
- `test.py` 从 `base.py` 导入，然后覆盖一些运行测试所需的特定配置。
  - 它应该被用作 `pytest.ini` 中的 settings module。
- `local.py` 从 `base.py` 导入，可以覆盖一些本地开发特定的配置。
  - 如果你想用它，就在 `manage.py` 中指向 `local`。否则就用 `base.py`。

在 `config/settings` 中，我们放所有其他配置：

- Celery 配置。
- 第三方服务配置。
- 等等。

这能给你带来清爽的模块划分。

另外，我们通常会有一个 `config/env.py`，内容如下：

```python
import environ

env = environ.Env()
```

然后，每当我们需要从环境读取什么，就这样导入：

```python
from config.env import env
```

通常在 `base.py` 模块的末尾，我们会从 `config/settings` 导入所有东西：

```python
from config.settings.cors import *  # noqa
from config.settings.sessions import *  # noqa
from config.settings.celery import *  # noqa
from config.settings.sentry import *  # noqa
```

### 给环境变量加上 `DJANGO_` 前缀

在很多示例中，你会看到环境变量通常都带有 `DJANGO_` 前缀。当你的 Django 应用与其他应用运行在同一环境中时，这非常有用。这种情况下，给环境变量加上 `DJANGO_` 前缀能帮你区分哪些是 Django 应用专属的环境变量。

在 HackSoft，我们通常不会在同一环境中运行多个应用。所以我们倾向于只给 Django 专属的环境变量加 `DJANGO_` 前缀，仅此而已。

例如，我们会给 `DJANGO_SETTINGS_MODULE`、`DJANGO_DEBUG`、`DJANGO_ALLOWED_HOSTS`、`DJANGO_CORS_ORIGIN_WHITELIST` 加前缀，而 `AWS_SECRET_KEY`、`CELERY_BROKER_URL`、`EMAILS_ENABLED` 则不加。

这主要取决于个人偏好。**只要确保你保持一致就行。**

### 第三方集成

既然所有配置都应该在 `base.py` 中被导入，但有时我们又不想在本地开发时配置某个集成，于是我们总结出以下做法：

- 集成专属的配置放在 `config/settings/some_integration.py`。
- 总是有一个名为 `USE_SOME_INTEGRATION` 的布尔配置，从环境中读取，默认为 `False`。
- 如果值为 `True`，就继续读取其他配置，并在环境变量缺失时直接报错。

举个例子，看看 `config/settings/sentry.py`：

```python
from config.env import env

SENTRY_DSN = env('SENTRY_DSN', default='')

if SENTRY_DSN:
    import sentry_sdk
    from sentry_sdk.integrations.django import DjangoIntegration
    from sentry_sdk.integrations.celery import CeleryIntegration

    # ... we proceed with sentry settings here ...
    # View the full file here - https://github.com/HackSoftware/Styleguide-Example/blob/master/config/settings/sentry.py
```

### 从 `.env` 读取

在本地放一个 `.env` 文件，是为配置提供值的好办法。

好消息是，[`django-environ`](https://django-environ.readthedocs.io/en/latest/) 提供了这样做的方法：

```python
# That's in the beginning of base.py

import os

from config.env import env, environ

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = environ.Path(__file__) - 3

env.read_env(os.path.join(BASE_DIR, ".env"))
```

现在你可以在项目根目录放一个 `.env` 文件（但不是必须的），把配置值放在里面。

这里有两点值得注意：

1. 不要把 `.env` 提交进版本控制，因为这会泄露凭据。
2. 而是放一个 `.env.example`，所有值都留空，这样新加入的开发者就能知道用到了哪些变量。

## 错误与异常处理

> 👀 如果你想要代码，请跳到 `Styleguide-Example` 项目 - <https://github.com/HackSoftware/Styleguide-Example/blob/master/styleguide_example/api/exception_handlers.py>

错误与异常处理是个大话题，而且很多时候——细节是因项目而异的。

所以我们会把事情分成两部分——**通用准则**，然后是一些**具体的错误处理方案**。

**我们的通用准则是：**

1. 了解异常处理的工作机制（我们会以 Django Rest Framework 为背景来讲）。
1. 定义你的 API 错误长什么样。
1. 掌握如何修改默认的异常处理行为。

**然后是一些具体方案：**

1. 沿用 DRF 默认异常，仅做少量修改。
1. HackSoft 推荐的方案。

如果你在寻找一种标准化的错误响应结构，**可以看看 RFC7807** - <https://datatracker.ietf.org/doc/html/rfc7807>（由这里提出 - <https://github.com/HackSoftware/Django-Styleguide/issues/133>）

### 异常处理的工作机制（在 DRF 语境下）

DRF 有一份关于异常处理机制的优秀指南，务必先读一读 - <https://www.django-rest-framework.org/api-guide/exceptions/>

另外，这里有一张清晰的流程概览图：

![Exception handler (1)](https://user-images.githubusercontent.com/387867/142426205-2c0356e6-ce20-425e-a811-072c3334edb0.png)

基本上，如果异常处理器无法处理某个异常并返回 `None`，就会产生一个未处理异常和 `500 Server Error`。这通常是好事，因为你不会把需要注意的错误静默吞掉。

**不过，有一些怪癖需要我们注意。**

#### DRF 的 `ValidationError`

例如，如果我们像这样简单地抛出一个 `rest_framework.exceptions.ValidationError`：

```python
from rest_framework import exceptions


def some_service():
    raise exceptions.ValidationError("Some message")
```

响应体会长这样：

```json
["Some message"]
```

这看起来有点奇怪，因为如果换成这样写：

```python
from rest_framework import exceptions


def some_service():
    raise exceptions.ValidationError({"error": "Some message"})
```

响应体就会变成这样：

```json
{
  "error": "Some message"
}
```

这就是我们传给 `ValidationError` 的 `detail`。但它和最初的数组是两种不同的数据结构。

现在，如果我们决定抛出另一个 DRF 内置异常：

```python
from rest_framework import exceptions


def some_service():
    raise exceptions.NotFound()
```

响应体会长这样：

```json
{
  "detail": "Not found."
}
```

这和 `ValidationError` 的行为完全不同，这可能会带来问题。

到目前为止，DRF 的默认行为可能返回：

- 一个数组。
- 一个字典。
- 一个特定的 `{"detail": "something"}` 结果。

**所以如果我们要用 DRF 的默认行为，就需要处理这种不一致性。**

#### Django 的 `ValidationError`

而且，DRF 的默认异常处理与 Django 的 `ValidationError` 配合得并不好。

这段代码：

```python
from django.core.exceptions import ValidationError as DjangoValidationError


def some_service():
    raise DjangoValidationError("Some error message")
```

会产生一个未处理异常，导致 `500 Server Error`。

如果这个 `ValidationError` 来自模型校验，也同样如此，例如：

```python
def some_service():
    user = BaseUser()
    user.full_clean()  # Throws ValidationError
    user.save()
```

这同样会导致 `500 Server Error`。

如果我们想让它像 `rest_framework.exceptions.ValidationError` 一样被处理，就需要实现自己的[自定义异常处理器](https://www.django-rest-framework.org/api-guide/exceptions/#custom-exception-handling)：

```python
from django.core.exceptions import ValidationError as DjangoValidationError

from rest_framework.views import exception_handler
from rest_framework.serializers import as_serializer_error
from rest_framework import exceptions


def custom_exception_handler(exc, ctx):
    if isinstance(exc, DjangoValidationError):
        exc = exceptions.ValidationError(as_serializer_error(exc))

    response = exception_handler(exc, ctx)

    # If unexpected error occurs (server error, etc.)
    if response is None:
        return response

    return response
```

这基本上是默认实现，只是加了这段代码：

```python
if isinstance(exc, DjangoValidationError):
    exc = exceptions.ValidationError(as_serializer_error(exc))
```

由于我们需要在 `django.core.exceptions.ValidationError` 和 `rest_framework.exceptions.ValidationError` 之间做映射，我们使用了 DRF 的 `as_serializer_error`——这正是 DRF 在序列化器内部做这种映射时用的东西。

有了它，Django 的 `ValidationError` 就能和 DRF 的异常处理器和谐相处了。

### 定义你的 API 错误长什么样

这非常重要，应该在任何项目中尽早完成。

本质上是约定好 API 错误的接口——**一个错误作为 API 响应会长什么样？**

这与项目高度相关，你可以从一些流行的 API 中寻找灵感：

- Stripe - <https://stripe.com/docs/api/errors>

举个例子，我们可能会决定错误长这样：

1. 用 `4**` 和 `5**` 状态码区分不同类型的错误。
1. 每个错误都是一个只带 `message` 键的字典，包含错误信息。

```json
{
  "message": "Some error message here"
}
```

这足够简单：

- `400` 用于校验错误。
- `401` 用于认证错误。
- `403` 用于权限错误。
- `404` 用于未找到错误。
- `429` 用于限流错误。
- `500` 用于服务器错误（要小心，不要静默吞掉导致 500 的异常，并且要始终把这类异常上报到 Sentry 之类的服务）

再说一次，这取决于你，并且因项目而异。**我们会在后面的具体方案中提出类似的东西。**

### 掌握如何修改默认的异常处理行为

这同样重要，因为当你决定了错误长什么样之后，你需要通过自定义异常处理来实现它。

在上文讨论 Django 的 `ValidationError` 时，我们已经给出过一个例子。

在下面的章节中，我们还会提供更多示例。

### 方案一 - 沿用 DRF 默认异常，仅做少量修改

DRF 的错误处理做得不错。如果最终结果总是一致的，那就更好了。我们要做的正是这些少量修改。

我们希望最终得到的错误永远长这样：

```json
{
  "detail": "Some error"
}
```

或者

```json
{
  "detail": ["Some error", "Another error"]
}
```

或者

```json
{
  "detail": { "key": "... some arbitrary nested structure ..." }
}
```

总之，确保我们永远得到一个带 `detail` 键的字典。

此外，我们还想处理 Django 的 `ValidationError`。

为了实现这一点，我们的自定义异常处理器长这样：

```python
from django.core.exceptions import ValidationError as DjangoValidationError, PermissionDenied
from django.http import Http404

from rest_framework.views import exception_handler
from rest_framework import exceptions
from rest_framework.serializers import as_serializer_error


def drf_default_with_modifications_exception_handler(exc, ctx):
    if isinstance(exc, DjangoValidationError):
        exc = exceptions.ValidationError(as_serializer_error(exc))

    if isinstance(exc, Http404):
        exc = exceptions.NotFound()

    if isinstance(exc, PermissionDenied):
        exc = exceptions.PermissionDenied()

    response = exception_handler(exc, ctx)

    # If unexpected error occurs (server error, etc.)
    if response is None:
        return response

    if isinstance(exc.detail, (list, dict)):
        response.data = {
            "detail": response.data
        }

    return response
```

我们某种程度上复刻了原始的异常处理器，以便之后能处理 `APIException`（寻找 `detail`）。

现在，我们跑一组测试：

代码：

```python
def some_service():
    raise DjangoValidationError("Some error message")
```

响应：

```json
{
  "detail": {
    "non_field_errors": ["Some error message"]
  }
}
```

---

代码：

```python
from django.core.exceptions import PermissionDenied

def some_service():
    raise PermissionDenied()
```

响应：

```json
{
  "detail": "You do not have permission to perform this action."
}
```

---

代码：

```python
from django.http import Http404

def some_service():
    raise Http404()
```

响应：

```json
{
  "detail": "Not found."
}
```

---

代码：

```python
def some_service():
    raise RestValidationError("Some error message")
```

响应：

```json
{
  "detail": ["Some error message"]
}
```

---

代码：

```python
def some_service():
    raise RestValidationError(detail={"error": "Some error message"})
```

响应：

```json
{
  "detail": {
    "error": "Some error message"
  }
}
```

---

代码：

```python
class NestedSerializer(serializers.Serializer):
    bar = serializers.CharField()


class PlainSerializer(serializers.Serializer):
    foo = serializers.CharField()
    email = serializers.EmailField(min_length=200)

    nested = NestedSerializer()


def some_service():
    serializer = PlainSerializer(data={
        "email": "foo",
        "nested": {}
    })
    serializer.is_valid(raise_exception=True)

```

响应：

```json
{
  "detail": {
    "foo": ["This field is required."],
    "email": [
      "Ensure this field has at least 200 characters.",
      "Enter a valid email address."
    ],
    "nested": {
      "bar": ["This field is required."]
    }
  }
}
```

---

代码：

```python
from rest_framework import exceptions


def some_service():
    raise exceptions.Throttled()
```

响应：

```json
{
  "detail": "Request was throttled."
}
```

---

代码：

```python
def some_service():
    user = BaseUser()
    user.full_clean()
```

响应：

```json
{
  "detail": {
    "password": ["This field cannot be blank."],
    "email": ["This field cannot be blank."]
  }
}
```

### 方案二 - HackSoft 推荐的做法

我们接下来要提出的方案，可以很容易地扩展成适合你的东西。

**核心思路如下：**

1. **你的应用会有自己的异常体系**，由业务逻辑抛出。
1. 简单起见，假设我们只定义 1 种错误——`ApplicationError`。
   - 它会被定义在一个专门的 `core` 应用的 `exceptions` 模块中。也就是 `project.core.exceptions.ApplicationError`。
1. 其他一切都交给 DRF 默认处理。
1. `ValidationError` 现在是特殊的，会被区别对待。
   - `ValidationError` 只应该来自序列化器校验或模型校验。

---

**我们为错误定义如下结构：**

```json
{
  "message": "The error message here",
  "extra": {}
}
```

`extra` 键可以存放任意数据，用于向前端传递信息。

例如，每当我们遇到一个 `ValidationError`（通常来自序列化器或模型），我们会这样呈现错误：

```json
{
  "message": "Validation error.",
  "extra": {
    "fields": {
      "password": ["This field cannot be blank."],
      "email": ["This field cannot be blank."]
    }
  }
}
```

这一点可以与前端沟通好，让他们去找 `extra.fields`，把这些具体的错误展示给用户。

为了实现它，自定义异常处理器长这样：

```python
from django.core.exceptions import ValidationError as DjangoValidationError, PermissionDenied
from django.http import Http404

from rest_framework.views import exception_handler
from rest_framework import exceptions
from rest_framework.serializers import as_serializer_error
from rest_framework.response import Response

from styleguide_example.core.exceptions import ApplicationError


def hacksoft_proposed_exception_handler(exc, ctx):
    """
    {
        "message": "Error message",
        "extra": {}
    }
    """
    if isinstance(exc, DjangoValidationError):
        exc = exceptions.ValidationError(as_serializer_error(exc))

    if isinstance(exc, Http404):
        exc = exceptions.NotFound()

    if isinstance(exc, PermissionDenied):
        exc = exceptions.PermissionDenied()

    response = exception_handler(exc, ctx)

    # If unexpected error occurs (server error, etc.)
    if response is None:
        if isinstance(exc, ApplicationError):
            data = {
                "message": exc.message,
                "extra": exc.extra
            }
            return Response(data, status=400)

        return response

    if isinstance(exc.detail, (list, dict)):
        response.data = {
            "detail": response.data
        }

    if isinstance(exc, exceptions.ValidationError):
        response.data["message"] = "Validation error"
        response.data["extra"] = {
            "fields": response.data["detail"]
        }
    else:
        response.data["message"] = response.data["detail"]
        response.data["extra"] = {}

    del response.data["detail"]

    return response
```

看看这段代码，试着理解它在做什么。**策略是——尽可能复用 DRF 的东西，然后再做调整。**

现在，我们会得到以下行为：

代码：

```python
from styleguide_example.core.exceptions import ApplicationError


def trigger_application_error():
    raise ApplicationError(message="Something is not correct", extra={"type": "RANDOM"})
```

响应：

```json
{
  "message": "Something is not correct",
  "extra": {
    "type": "RANDOM"
  }
}
```

---

代码：

```python
def some_service():
    raise DjangoValidationError("Some error message")
```

响应：

```json
{
  "message": "Validation error",
  "extra": {
    "fields": {
      "non_field_errors": ["Some error message"]
    }
  }
}
```

---

代码：

```python
from django.core.exceptions import PermissionDenied

def some_service():
    raise PermissionDenied()
```

响应：

```json
{
  "message": "You do not have permission to perform this action.",
  "extra": {}
}
```

---

代码：

```python
from django.http import Http404

def some_service():
    raise Http404()
```

响应：

```json
{
  "message": "Not found.",
  "extra": {}
}
```

---

代码：

```python
def some_service():
    raise RestValidationError("Some error message")
```

响应：

```json
{
  "message": "Validation error",
  "extra": {
    "fields": ["Some error message"]
  }
}
```

---

代码：

```python
def some_service():
    raise RestValidationError(detail={"error": "Some error message"})
```

响应：

```json
{
  "message": "Validation error",
  "extra": {
    "fields": {
      "error": "Some error message"
    }
  }
}
```

---

代码：

```python
class NestedSerializer(serializers.Serializer):
    bar = serializers.CharField()


class PlainSerializer(serializers.Serializer):
    foo = serializers.CharField()
    email = serializers.EmailField(min_length=200)

    nested = NestedSerializer()


def some_service():
    serializer = PlainSerializer(data={
        "email": "foo",
        "nested": {}
    })
    serializer.is_valid(raise_exception=True)

```

响应：

```json
{
  "message": "Validation error",
  "extra": {
    "fields": {
      "foo": ["This field is required."],
      "email": [
        "Ensure this field has at least 200 characters.",
        "Enter a valid email address."
      ],
      "nested": {
        "bar": ["This field is required."]
      }
    }
  }
}
```

---

代码：

```python
from rest_framework import exceptions


def some_service():
    raise exceptions.Throttled()
```

响应：

```json
{
  "message": "Request was throttled.",
  "extra": {}
}
```

---

代码：

```python
def some_service():
    user = BaseUser()
    user.full_clean()
```

响应：

```json
{
  "message": "Validation error",
  "extra": {
    "fields": {
      "password": ["This field cannot be blank."],
      "email": ["This field cannot be blank."]
    }
  }
}
```

---

这个方案可以进一步扩展，变得更适合你的需求：

1. 你可以定义 `ApplicationValidationError` 和 `ApplicationPermissionError`，作为额外的异常层级。
1. 你可以重新实现 DRF 的默认异常处理器，而不是复用它（复制粘贴过来，按你的需求调整）。

**总体思路是——先想清楚你需要什么样的错误处理，然后照着实现。**

### 更多思路

如你所见，异常处理可以被塑造成我们需要的样子。

你可以开始处理更多东西——例如——把 `django.core.exceptions.ObjectDoesNotExist` 转换成 `rest_framework.exceptions.NotFound`。

你甚至可以处理所有异常，但那样的话，你必须确保这些异常被妥善记录，否则你可能会把重要的东西静默吞掉。

## 测试

### 概览

测试是个有趣且庞大的话题。

作为概览，你可以观看 [Radoslav Georgiev 在 DjangoCon Europe 2022 上的演讲](https://www.youtube.com/watch?v=PChaEAIsQls)：

[![Quality Assurance in Django - Testing what matters](https://img.youtube.com/vi/PChaEAIsQls/0.jpg)](https://www.youtube.com/watch?v=PChaEAIsQls)

在我们的 Django 项目中，我们根据被测代码的类型来划分测试。

也就是说，我们通常会为模型、服务、选择器和 API / 视图分别编写测试。

文件结构通常长这样：

```
project_name
├── app_name
│   ├── __init__.py
│   └── tests
│       ├── __init__.py
│       ├── factories.py
│       ├── models
│       │   └── __init__.py
│       │   └── test_some_model_name.py
│       ├── selectors
│       │   └── __init__.py
│       │   └── test_some_selector_name.py
│       └── services
│           ├── __init__.py
│           └── test_some_service_name.py
└── __init__.py
```

### 命名约定

我们遵循 2 条通用的命名约定：

- 测试文件名应该是 `test_the_name_of_the_thing_that_is_tested.py`（test_ + 被测事物的名称）
- 测试用例类应该是 `class TheNameOfTheThingThatIsTestedTests(TestCase):`（被测事物的名称 + Tests）

例如，如果我们有：

```python
def a_very_neat_service(*args, **kwargs):
    pass
```

那么文件名应该是：

```
project_name/app_name/tests/services/test_a_very_neat_service.py
```

测试用例类应该是：

```python
class AVeryNeatServiceTests(TestCase):
    pass
```

对于工具函数的测试，我们遵循类似的模式。

例如，如果我们有 `project_name/common/utils.py`，那么我们会有 `project_name/common/tests/test_utils.py`，把不同的测试用例放在那个文件里。

如果我们要把 `utils.py` 模块拆分成子模块，测试也会做同样的拆分：

- `project_name/common/utils/files.py`
- `project_name/common/tests/utils/test_files.py`

我们尽量让模块的结构与对应测试的结构保持一致。

### 测试工厂（Factories）

工厂是为测试生成数据的好工具。

使用得当的话，它可以提升测试的整体质量。

如果你刚接触这个概念，可以参考以下资料：

- [Improve your Django tests with fakes and factories](https://www.hacksoft.io/blog/improve-your-tests-django-fakes-and-factories)
- [https://www.hacksoft.io/blog/improve-your-tests-django-fakes-and-factories-advanced-usage](https://www.hacksoft.io/blog/improve-your-tests-django-fakes-and-factories-advanced-usage)
- [DjangoCon 2022 | factory_boy: testing like a pro](https://www.youtube.com/watch?v=-C-XNHAJF-c)

## Celery

我们在以下通用场景中使用 [Celery](http://www.celeryproject.org/)：

- 与第三方服务通信（发送邮件、通知等）
- 把较重的计算任务从 HTTP 周期中卸载出去。
- 定时任务（使用 Celery beat）

### 基本原则

我们尽量把 Celery 当作通往核心逻辑的又一个接口——也就是说——**不要在任务里放业务逻辑。**

我们来看一个发送邮件的**服务**示例（示例取自 [`Django-Styleguide-Example`](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/emails/tasks.py)）：

```python
from django.db import transaction
from django.core.mail import EmailMultiAlternatives

from styleguide_example.core.exceptions import ApplicationError
from styleguide_example.common.services import model_update
from styleguide_example.emails.models import Email


@transaction.atomic
def email_send(email: Email) -> Email:
    if email.status != Email.Status.SENDING:
        raise ApplicationError(f"Cannot send non-ready emails. Current status is {email.status}")

    subject = email.subject
    from_email = "styleguide-example@hacksoft.io"
    to = email.to

    html = email.html
    plain_text = email.plain_text

    msg = EmailMultiAlternatives(subject, plain_text, from_email, [to])
    msg.attach_alternative(html, "text/html")

    msg.send()

    email, _ = model_update(
        instance=email,
        fields=["status", "sent_at"],
        data={
            "status": Email.Status.SENT,
            "sent_at": timezone.now()
        }
    )
    return email
```

发送邮件这件事带有业务逻辑，**但我们仍然希望从一个任务中触发这个特定的服务。**

我们的任务长这样：

```python
from celery import shared_task

from styleguide_example.emails.models import Email


@shared_task
def email_send(email_id):
    email = Email.objects.get(id=email_id)

    from styleguide_example.emails.services import email_send
    email_send(email)
```

如你所见，**我们把任务当作一个 API 来对待：**

1. 获取所需数据。
2. 调用相应的服务。

现在，想象我们有另一个服务，它会触发邮件发送。

它可能长这样：

```python
from django.db import transaction

# ... more imports here ...

from styleguide_example.emails.tasks import email_send as email_send_task


@transaction.atomic
def user_complete_onboarding(user: User) -> User:
    # ... some code here

    email = email_get_onboarding_template(user=user)

    transaction.on_commit(lambda: email_send_task.delay(email.id))

    return user
```

这里有两点需要特别指出：

1. 我们导入了任务（它和服务同名），但给它加了 `_task` 后缀。
1. 当事务提交时，我们会调用这个任务。

**所以，总的来说，我们使用 Celery 的方式可以描述为：**

1. 任务调用服务。
2. 我们在任务的函数体内导入服务。
3. 当我们想触发一个任务时，在模块级别导入任务，并加上 `_task` 后缀。
4. 我们在事务提交时，以副作用（side effect）的方式执行任务。

这种把任务与服务混合使用的方式还能**避免循环导入**——使用 Celery 时循环导入相当常见。

### 错误处理

有时候，服务会失败，而我们可能想在任务层面处理错误。例如——我们可能想重试任务。

这些错误处理代码需要写在任务里。

我们来扩展上面的 `email_send` 任务示例，加上错误处理：

```python
from celery import shared_task
from celery.utils.log import get_task_logger

from styleguide_example.emails.models import Email


logger = get_task_logger(__name__)


def _email_send_failure(self, exc, task_id, args, kwargs, einfo):
    email_id = args[0]
    email = Email.objects.get(id=email_id)

    from styleguide_example.emails.services import email_failed

    email_failed(email)


@shared_task(bind=True, on_failure=_email_send_failure)
def email_send(self, email_id):
    email = Email.objects.get(id=email_id)

    from styleguide_example.emails.services import email_send

    try:
        email_send(email)
    except Exception as exc:
        # https://docs.celeryq.dev/en/stable/userguide/tasks.html#retrying
        logger.warning(f"Exception occurred while sending email: {exc}")
        self.retry(exc=exc, countdown=5)
```

如你所见，我们会做多次重试，如果全部失败，就在 `on_failure` 回调中处理。

这个回调遵循 `_{task_name}_failure` 的命名模式，并且像普通任务一样调用服务层。

### 配置

我们基本遵循 Celery 与 Django 集成的官方指南 - <https://docs.celeryq.dev/en/stable/django/first-steps-with-django.html>

完整的例子可以查看 `Django-Styleguide-Example` 项目中的 Celery 配置：

- <https://github.com/HackSoftware/Django-Styleguide-Example/tree/master/styleguide_example/tasks>
- <https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/tasks/celery.py>

Celery 是个复杂的话题，所以值得投入时间阅读文档、理解不同的配置项。

我们也一直在这样做，并不断发现新东西、找到解决问题的更好方法。

### 结构

任务位于各个应用的 `tasks.py` 模块中。

我们遵循与其他一切（API、服务、选择器）相同的规则：**如果某个应用的任务变得太大，就按领域拆分。**

也就是说，你最终会得到 `tasks/domain_a.py` 和 `tasks/domain_b.py`。你需要做的只是在 `tasks/__init__.py` 中导入它们，以便 Celery 能自动发现。

总体经验法则是——以对你有意义的方式拆分任务。

### 定时任务

管理定时任务相当重要，尤其是当你有几十甚至上百个定时任务时。

我们的定时任务使用 [Celery Beat](https://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html) + `django_celery_beat.schedulers:DatabaseScheduler` + [`django-celery-beat`](https://github.com/celery/django-celery-beat)。

我们额外做的一件事是编写一个名为 [`setup_periodic_tasks`](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/tasks/management/commands/setup_periodic_tasks.py) 的管理命令（management command），它保存着系统中所有定时任务的定义。这个命令位于上文提到的 `tasks` 应用中。

`project.tasks.management.commands.setup_periodic_tasks.py` 长这样：

```python
from django.core.management.base import BaseCommand
from django.db import transaction

from django_celery_beat.models import IntervalSchedule, CrontabSchedule, PeriodicTask

from project.app.tasks import some_periodic_task


class Command(BaseCommand):
    help = f"""
    Setup celery beat periodic tasks.

    Following tasks will be created:

    - {some_periodic_task.name}
    """

    @transaction.atomic
    def handle(self, *args, **kwargs):
        print('Deleting all periodic tasks and schedules...\n')

        IntervalSchedule.objects.all().delete()
        CrontabSchedule.objects.all().delete()
        PeriodicTask.objects.all().delete()

        periodic_tasks_data = [
            {
                'task': some_periodic_task
                'name': 'Do some peridoic stuff',
                # https://crontab.guru/#15_*_*_*_*
                'cron': {
                    'minute': '15',
                    'hour': '*',
                    'day_of_week': '*',
                    'day_of_month': '*',
                    'month_of_year': '*',
                },
                'enabled': True
            },
        ]

        for periodic_task in periodic_tasks_data:
            print(f'Setting up {periodic_task["task"].name}')

            cron = CrontabSchedule.objects.create(
                **periodic_task['cron']
            )

            PeriodicTask.objects.create(
                name=periodic_task['name'],
                task=periodic_task['task'].name,
                crontab=cron,
                enabled=periodic_task['enabled']
            )
```

几个关键点：

- 我们把这个命令作为部署流程的一部分。
- 我们总是附上一个 [`crontab.guru`](https://crontab.guru) 的链接来解释 cron 表达式，否则它根本没法读。
- 所有东西都集中在一个地方。
- ⚠️ 我们几乎只使用 crontab 形式的调度。**如果你打算使用 Celery 提供的其他调度对象，请务必通读它们的文档**以及重要提示 - <https://django-celery-beat.readthedocs.io/en/latest/#example-creating-interval-based-periodic-task> - 关于多个任务指向同一个调度对象的注意事项。⚠️

### 进阶

Celery 有强大的工具来实现复杂的工作流 - <https://docs.celeryq.dev/en/stable/userguide/canvas.html>

如果你决定使用它们，上面的规则依然适用。

你可能需要重新组织一些东西，但只要你为应用核心定义了清晰的接口，你就能在更复杂的场景中自由组合任务与服务。

**更复杂的场景取决于具体上下文。确保你清楚架构以及自己正在做的决策。**

## Cookbook 实战手册

一些通用可复用代码片段的实现存放在这里。

### 用服务（service）处理更新

对于更新操作，我们有一个通用的更新服务，在实际的更新服务内部使用。下面是一个 `user_update` 服务的示例：

```python
def user_update(*, user: User, data) -> User:
    non_side_effect_fields = ['first_name', 'last_name']

    user, has_updated = model_update(
        instance=user,
        fields=non_side_effect_fields,
        data=data
    )

    # Side-effect fields update here (e.g. username is generated based on first & last name)

    # ... some additional tasks with the user ...

    return user
```

- 对于那些没有副作用的字段（也就是直接设置为我们提供的值即可的字段），我们调用通用的 `model_update` 服务。
- 这个模式让我们能把重复的字段赋值抽离到通用服务中，只在更新服务里做特定的事情（副作用）。
- 我们还可以更聪明一点：保存实例时提供 `update_fields` 关键字参数。这样，在 `UPDATE` 查询中，我们只发送真正被更新的值。

这些服务的完整实现可以在我们的示例项目中找到：

- [`model_update`](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/common/services.py)
- [`user_update`](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/users/services.py)

如果你打算在项目中引入 `model_update`，务必[阅读它的测试](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/common/tests/services/test_model_update.py)，并把测试也一并带上！

## DX（开发者体验）

这一节收集了各种能让你的 Django 开发生活更美好的东西。

### `mypy` 与类型注解

关于 [`mypy`](https://mypy.readthedocs.io/en/stable/index.html)，我们的理念是：

> 如果它对你有意义、能帮你产出更好的软件，那就用它。

在 HackSoft，我们有：

- 强制使用 `mypy` 且非常严格的项目。
- 类型比较宽松、完全不用 `mypy` 的项目。

上下文为王。

在 [`Django-Styleguide-Example`](https://github.com/HackSoftware/Django-Styleguide-Example) 中，我们配置了 `mypy`，同时使用了 <https://github.com/typeddjango/django-stubs> 和 <https://github.com/typeddjango/djangorestframework-stubs/>。你可以把它当作示例参考。

另外，这个项目 - <https://github.com/wemake-services/wemake-django-template> - 也有 `mypy` 配置。

弄清楚什么最适合你。

## Django Styleguide 的实际应用案例

这里收集了一些觉得这份风格指南有用的个人和公司：

---

**Michael Valencia，[Facturedo](https://facturedo.com/) CTO**

> 我们 Facturedo 核心项目的源代码开始变得混乱。
> 业务逻辑散落在许多互不连贯的地方。我们需要一个组织 Django 项目的方案，并在 Django Styleguide 中找到了它。
>
> 我们把它推荐给任何想要组织中大型项目的人。
> 这是一份定义完善、且在不断演进的指南。

---

## 更多资源与替代方案

我们觉得有用的其他资源和替代方案，可以为这份风格指南增值：

- [Dan Palmer - Scaling Django to 500 apps (DjangoCon US 2021)](https://www.youtube.com/watch?v=NsHo-kThlqI)
- [Django API Domains](https://phalt.github.io/django-api-domains/)
- [Y Combinator News 上围绕 Django Styleguide 的讨论](https://news.ycombinator.com/item?id=34337667)——你可能会在这里发现更多有用的东西。

## 灵感来源

我们构建 Django 应用的方式，受以下事物的启发：

- **关注点分离**的总体理念
- [Boundaries by Gary Bernhardt](https://www.youtube.com/watch?v=yTkzNHF6rMs)
- Rails 的服务对象（service objects）
- 最近，我看到了 [Cognitive Load is what matters](https://github.com/zakirullin/cognitive-load) 这篇文章，深有共鸣。其中提到的一些东西也是 Django Styleguide 的关键理念。
