ビルド
```shell
docker-compose build
```

起動
```bash
docker-compose up -d
```

Google Cloud設定（方法１）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

settings.py
```python
# 環境変数の設定
import os
os.environ["GCLOUD_PROJECT"] = "your-project-id"
```

wsgi.py
```python
# プロファイリングの開始
import google_cloud_profiler

# Googleクラウドプロジェクト ID
try:
    # プロファイリングの開始
    google_cloud_profiler.start(
        service="your-service-name",
        service_version="1.0",
        verbose=2,
    )
except (ValueError, NotImplementedError) as exp:
    logger.error("Could not start Google Cloud Profiler Python agent: %s", exp)

# Django の通常の設定

# アプリケーションの終了時にプロファイリングを停止
google_cloud_profiler.stop()
```

結果
```shell
Exception in thread django-main-thread:
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/dist-packages/django/core/servers/basehttp.py", line 48, in get_internal_wsgi_application
    return import_string(app_path)
           ^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 30, in import_string
    return cached_import(module_path, class_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 15, in cached_import
    module = import_module(module_path)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1149, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/root/django/my_project/my_project/wsgi.py", line 11, in <module>
    import google_cloud_profiler
ModuleNotFoundError: No module named 'google_cloud_profiler'
```


Google Cloud設定（方法2）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

個別関数修正
```python
from google.cloud import profiler

# プロファイラを有効にする
profiler.start_fireprofiler('your-project-id')

# プロファイラを無効にする
profiler.stop_fireprofiler('your-project-id')
```

結果
```shell
Exception in thread django-main-thread:
Traceback (most recent call last):
  File "/usr/lib/python3.11/threading.py", line 1038, in _bootstrap_inner
    self.run()
  File "/usr/lib/python3.11/threading.py", line 975, in run
    self._target(*self._args, **self._kwargs)
  File "/usr/local/lib/python3.11/dist-packages/django/utils/autoreload.py", line 64, in wrapper
    fn(*args, **kwargs)
  File "/usr/local/lib/python3.11/dist-packages/django/core/management/commands/runserver.py", line 133, in inner_run
    self.check(display_num_errors=True)
  File "/usr/local/lib/python3.11/dist-packages/django/core/management/base.py", line 485, in check
    all_issues = checks.run_checks(
                 ^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/checks/registry.py", line 88, in run_checks
    new_errors = check(app_configs=app_configs, databases=databases)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/checks/urls.py", line 42, in check_url_namespaces_unique
    all_namespaces = _load_all_namespaces(resolver)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/checks/urls.py", line 61, in _load_all_namespaces
    url_patterns = getattr(resolver, "url_patterns", [])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/functional.py", line 57, in __get__
    res = instance.__dict__[self.name] = self.func(instance)
                                         ^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/urls/resolvers.py", line 715, in url_patterns
    patterns = getattr(self.urlconf_module, "urlpatterns", self.urlconf_module)
                       ^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/functional.py", line 57, in __get__
    res = instance.__dict__[self.name] = self.func(instance)
                                         ^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/urls/resolvers.py", line 708, in urlconf_module
    return import_module(self.urlconf_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1149, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/root/django/my_project/my_project/urls.py", line 23, in <module>
    path('', include('simple.urls')),
             ^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/urls/conf.py", line 38, in include
    urlconf_module = import_module(urlconf_module)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1149, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/root/django/my_project/simple/urls.py", line 3, in <module>
    from . import views
  File "/root/django/my_project/simple/views.py", line 2, in <module>
    from google.cloud import profiler
```

Google Cloud設定（方法３）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```
settings.py
```python
# settings.py

# Google Cloud Profilerを有効にする
CLOUD_PROFILER_ENABLED = True

# Google Cloud Profilerの設定
if CLOUD_PROFILER_ENABLED:
    MIDDLEWARE.insert(0, 'googlecloudprofiler.middleware.ProfilerMiddleware')

    # Google Cloud Profilerの設定
    GOOGLE_CLOUD_PROFILER = {
        'service_name': 'your-service-name',  # サービス名を指定
        'service_version': 'your-service-version',  # サービスのバージョンを指定
        # ローカルでテストする際には、Falseに設定
        'ignore_profiler_middleware': False,  
    }

```

権限を付与
```shell
gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
    --member=serviceAccount:YOUR_PROJECT_NUMBER-compute@developer.gserviceaccount.com \
    --role=roles/cloudprofiler.agent

```

結果
```shell
Exception in thread django-main-thread:
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/dist-packages/django/core/servers/basehttp.py", line 48, in get_internal_wsgi_application
    return import_string(app_path)
           ^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 30, in import_string
    return cached_import(module_path, class_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 15, in cached_import
    module = import_module(module_path)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1149, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/root/django/my_project/my_project/wsgi.py", line 16, in <module>
    application = get_wsgi_application()
                  ^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/wsgi.py", line 13, in get_wsgi_application
    return WSGIHandler()
           ^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/wsgi.py", line 118, in __init__
    self.load_middleware()
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/base.py", line 40, in load_middleware
    middleware = import_string(middleware_path)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 30, in import_string
    return cached_import(module_path, class_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/module_loading.py", line 15, in cached_import
    module = import_module(module_path)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1142, in _find_and_load_unlocked
ModuleNotFoundError: No module named 'googlecloudprofiler.middleware'
```

Google Cloud設定（方法4）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

全部のURL Pathに対応する場合
settings.py
```python
import googlecloudprofiler

googlecloudprofiler.start(service="my-service", service_version="1.0.0", service_account_json_file="/path/to/service-account-file.json")

```

IAM権限追加
```shell
# Google Cloud Consoleにアクセス
https://console.cloud.google.com/

# IAM と管理 > IAM > 該当ユーザの権限に以下を追加する
Cloud Profiler Agent
```

Google Cloud設定（方法5）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

本番環境では、全体的に適用するとパフォーマンス影響あるため、Top画面のみ適用
```python
# views.py
import googlecloudprofiler

def index(request):
    # top pageの処理

    # プロファイリングを開始する
    googlecloudprofiler.start(
        service="my-service",
        service_version="1.0.0",
        service_account_json_file="/path/to/service-account-file.json"
    )

    return HttpResponse('Hello World')

```

結果
```shell
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/root/django/my_project/simple/views.py", line 6, in index
    googlecloudprofiler.start(service="staging", service_version="27", service_account_json_file="/root/django/my_project/my_project/gcs-auth.json")
  File "/usr/local/lib/python3.11/dist-packages/googlecloudprofiler/__init__.py", line 131, in start
    profiler_client.start()
  File "/usr/local/lib/python3.11/dist-packages/googlecloudprofiler/client.py", line 224, in start
    self._profilers['WALL'].register_handler()
  File "/usr/local/lib/python3.11/dist-packages/googlecloudprofiler/pythonprofiler.py", line 98, in register_handler
    signal.signal(signal.SIGALRM, self._handler)
  File "/usr/lib/python3.11/signal.py", line 56, in signal
    handler = _signal.signal(_enum_to_int(signalnum), _enum_to_int(handler))
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ValueError: signal only works in main thread of the main interpreter

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/deprecation.py", line 134, in __call__
    response = response or self.get_response(request)
                           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/exception.py", line 57, in inner
    response = response_for_exception(request, exc)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/exception.py", line 140, in response_for_exception
    response = handle_uncaught_exception(
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/core/handlers/exception.py", line 181, in handle_uncaught_exception
    return debug.technical_500_response(request, *exc_info)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/views/debug.py", line 67, in technical_500_response
    html = reporter.get_traceback_html()
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/views/debug.py", line 411, in get_traceback_html
    return t.render(c)
           ^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 175, in render
    return self._render(context)
           ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 167, in _render
    return self.nodelist.render(context)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 1005, in render
    return SafeString("".join([node.render_annotated(context) for node in self]))
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 1005, in <listcomp>
    return SafeString("".join([node.render_annotated(context) for node in self]))
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 966, in render_annotated
    return self.render(context)
           ^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 1064, in render
    output = self.filter_expression.resolve(context)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/template/base.py", line 738, in resolve
    obj = template_localtime(obj, context.use_tz)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/timezone.py", line 196, in template_localtime
    return localtime(value) if should_convert else value
           ^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/timezone.py", line 215, in localtime
    timezone = get_current_timezone()
               ^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/timezone.py", line 96, in get_current_timezone
    return getattr(_active, "value", get_default_timezone())
                                     ^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/dist-packages/django/utils/timezone.py", line 82, in get_default_timezone
    return zoneinfo.ZoneInfo(settings.TIME_ZONE)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/zoneinfo/_common.py", line 24, in load_tzdata
    raise ZoneInfoNotFoundError(f"No time zone found with key {key}")
zoneinfo._common.ZoneInfoNotFoundError: 'No time zone found with key Asia/Tokyo'
```

Google Cloud設定（方法6 ）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

wsgi.py
```python
import os
import googlecloudprofiler
from django.core.wsgi import get_wsgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'my_project.settings')

googlecloudprofiler.start(service="staging", service_version="27", service_account_json_file="/root/django/my_project/my_project/gcs-auth.json")

application = get_wsgi_application()

```

結果
```shell
例外が発生しました: ValueError
signal only works in main thread of the main interpreter
  File "/root/django/my_project/my_project/wsgi.py", line 16, in <module>
    googlecloudprofiler.start(service="staging", service_version="27", service_account_json_file="/root/django/my_project/my_project/gcs-auth.json")
ValueError: signal only works in main thread of the main interpreter
```

Google Cloud設定（方法7）
```shell
# Cloud Profiler ライブラリのインストール
pip install google-cloud-profiler
pip freeze | grep google-cloud-profiler
```

middleware.py
```python
from googlecloudprofiler import start

class CloudProfilerMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
        start(service="my-service", service_version="1.0.0", service_account_json_file="/path/to/service-account-file.json")

    def __call__(self, request):
        response = self.get_response(request)
        return response

```

settings.py
```python
MIDDLEWARE = [
    # 他のミドルウェアを追加
    'your_app.middleware.CloudProfilerMiddleware',
]

```
結果
```shell
例外が発生しました: ValueError
signal only works in main thread of the main interpreter
  File "/root/django/my_project/simple/middleware.py", line 8, in __init__
    start(service="staging", service_version="27", service_account_json_file="/root/django/my_project/my_project/gcs-auth.json")
  File "/root/django/my_project/my_project/wsgi.py", line 16, in <module>
    application = get_wsgi_application()
                  ^^^^^^^^^^^^^^^^^^^^^^
ValueError: signal only works in main thread of the main interpreter
```


動作確認
```shell
http://localhost:8001/
```