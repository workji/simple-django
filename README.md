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


動作確認
```shell
http://localhost:8001/
```