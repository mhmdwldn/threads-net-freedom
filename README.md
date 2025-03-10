META SHALL NOT SILENCE US, ORIGINAL WORK BY dmytrostriletskyi
 - Explanation here https://github.com/dmytrostriletskyi/threads-net
Unofficial and reverse-engineered Threads ([threads.net](https://www.threads.net/@zuck)) Python API wrapper. 

**Created for academic purposes and is not intended to be used in real software. Usage of it might violate `Instagram` 
and `Threads` terms. In addition to breaching the terms of service and interfering with Meta’s business expectations
and interests, your activities may violate other federal and state laws. See Computer Fraud and Abuse Act, 18 U.S.C. 
§ 1030; and the California Comprehensive Computer Data Access and Fraud Act, Cal. Penal Code § 502(c).**

[![](https://github.com/dmytrostriletskyi/threads-net/actions/workflows/main.yaml/badge.svg?branch=main)](https://github.com/dmytrostriletskyi/threads-net/actions/workflows/main.yaml)
[![](https://img.shields.io/github/release/dmytrostriletskyi/threads-net.svg)](https://github.com/dmytrostriletskyi/threads-net/releases)
[![](https://img.shields.io/pypi/v/threads-net.svg)](https://pypi.python.org/pypi/threads-net)

[![](https://pepy.tech/badge/threads-net)](https://pepy.tech/project/threads-net)
[![](https://img.shields.io/pypi/l/threads-net.svg)](https://pypi.python.org/pypi/threads-net/)
[![](https://img.shields.io/pypi/pyversions/threads-net.svg)](https://pypi.python.org/pypi/threads-net/)

Table of content:

* [Disclaimer](#disclaimer)
* [Roadmap](#roadmap)
* [Getting started](#getting-started)
  * [How to install](#how-to-install)
  * [Examples](#examples)
* [API](#api)
  * [Disclaimer](#disclaimer-1)
  * [Troubleshooting](#troubleshooting)
    * [Two-Factor Authentication (2FA)](#two-factor-authentication-2fa)
  * [Terminology](#terminology)
  * [Initialization](#initialization)
  * [Settings](#settings)
  * [Pagination](#pagination)
  * [Public](#api)
    * [User](#user)
      * [Get Identifier](#get-identifier)
      * [Get By Identifier](#get-by-identifier)
      * [Get Threads](#get-threads)
      * [Get Replies](#get-replies)
    * [Thread](#thread)
      * [Get Identifier](#get-identifier-1)
      * [Get By Identifier](#get-by-identifier-1)
      * [Get Likers](#get-likers)
  * [Private](#private)
    * [User](#user-1)
      * [Get Identifier](#get-identifier-2)
      * [Get By Identifier](#get-by-identifier-2)
      * [Get Threads](#get-threads-1)
      * [Get Replies](#get-replies-1)
      * [Get Followers](#get-followers)
      * [Get Following](#get-following)
      * [Get Friendship Status](#get-friendship-status)
      * [Search](#search)
      * [Follow](#follow)
      * [Unfollow](#unfollow)
      * [Mute](#mute)
      * [Unmute](#unmute)
      * [Restrict](#restrict)
      * [Unrestrict](#unrestrict)
      * [Block](#block)
      * [Unblock](#unblock)
    * [Thread](#thread-1)
      * [Get Identifier](#get-identifier-3)
      * [Get By Identifier](#get-by-identifier-3)
      * [Get Likers](#get-likers-1)
      * [Create](#create)
      * [Delete](#delete)
      * [Like](#like)
      * [Unlike](#unlike)
      * [Repost](#repost)
      * [Unrepost](#unrepost)
      * [Quote](#quote)
    * [Feed](#feed)
      * [Recommended users](#get-recommended-users)

## Disclaimer

**Created for academic purposes and is not intended to be used in real software. Usage of it might violate `Instagram` 
and `Threads` terms. In addition to breaching the terms of service and interfering with Meta’s business expectations
and interests, your activities may violate other federal and state laws. See Computer Fraud and Abuse Act, 18 U.S.C. 
§ 1030; and the California Comprehensive Computer Data Access and Fraud Act, Cal. Penal Code § 502(c).**

## Roadmap

Check what is already done in the table of content above, below the only things to be done are placed: 

- [ ] Pagination for all methods responding with a list of records
- [ ] Get notifications about new threads
- [ ] Manage auth for accounts with the enabled 2FA
- [ ] Manage auth for accounts required a challenge

## Getting started

### How to install

Install the library with the following command using `pip3`:

```bash
$ pip3 install threads-net
```

### Examples

Find examples of how to use the library in the `examples` folder:

```bash
ls examples
├── private
│   ├── create_thread.py
│   ├── follow_user.py
│   ├── get_user.py
│   ├── get_user_followers.py
│   ├── get_user_following.py
│   └── search_user.py
│   ...
└── public
    ├── get_thread.py
    ├── get_thread_likers.py
    ├── get_user.py
    ├── get_user_replies.py
    └── get_user_threads.py
    ...
```

## API

### Disclaimer

To interact with `Threads API`, there is no need for any authorization, so in the library it is called `public API`.
On the other hand, to interact with `Instagram API`, you have to specify `Instagram` username and password (as like
you do when you log in to `Threads` mobile application), so in the library it is called `private API`.

* `Public API` has only read-only endpoints. In the same time, `private API` has both read and write endpoints. 
  `Public API` much less stable than `private API` (because `private API` is used for `Instagram API` and `Instagram` 
  is stable product in general already).
* `Public API` less risky to get `rate limits` or to have an account in `Threads` and/or `Instagram` be suspended.

So, it is a trade-off between stability, bugs and `rate limits` and/or `suspension`. But with the library you can
combine two approaches: for instance, prefer `public API` for read-only endpoints and `private API` for write-only
endpoints or even build a retry mechanism (try public one, if failed, try private one).

### Troubleshooting

#### Two-Factor Authentication (2FA)

Currently, it is not possible to use `private API` with two-factor authentication enabled on the account. If you do not
disable it, you might face issues like [#37](https://github.com/dmytrostriletskyi/threads-net/issues/37). So, in order
to use `private API`, you have to disable it first. By the way, there are plans in the roadmap to manage authentication
for such accounts so you don't have to disable it.

### Terminology

There might be a confusion among many `Threads API` clients as well as in both `Threads` an `Instagram` `APIs` according
to the naming of entities. For instance, in `Threads` a publication is called a `thread`, but under the hood in the `API`
of fetching or creating a `thread` it is called a `post`. 

It is done because `Threads` are backed by `Instagram` and `threads` creation is done on `Instagram API` where a 
publication called a post. Library maintainers decided to stick into `Threads` terminology and use the word `thread` for
the post-related things.

### Initialization

Import the class responsible for `Threads API` communication and initialize the object.

If you are going to use only `public API`, use the following commands:

```python3
>>> from threads import Threads
>>> threads = Threads()
```

If you are going to use only `private API` or both `private` and `public` `APIs`, use the following commands:

```python3
>>> from threads import Threads
>>> threads = Threads(username='instagram_username', password='instagram_password')
```

### Settings

Currently, to perform `private API` requests, there is a need to specify a mobile device metadata, timezone offset and
an authorization token. The device metadata and timezone offset are randomly generated once you initialize `Threads`, 
in order the authorization token is requested from `Instagram API` and saved for further usage.

If you initialize `Threads` frequently with different data, `Instagram API` might threat it as a non-human and machine
behavior and mark your `IP` as suspicious and even suspend your account `Threads` and/or `Instagram` account. Also,
the authorization token has long live period and once the authorization token is fetched, it can be reused at least for 
next 24 hours (maybe more, but at least) and there is no need to fetch it at each initialization over and over again.

For this, there are settings. Those are either a `JSON` file or a dictionary you can provide to reuse device metadata, 
timezone offset and the authorization token instead of generating random data each initialization.

To provide settings as a dictionary, use the following commands:

```python
>>> settings = {
        'authentication': {
            'token': 'eyJkc191c2VyX2lkIjYnNjA0MzgxNTQ1N1NjYiLCJzZXNzaW9uaWQiOi1FxSUQ1TXcifQ==',
        },
        'timezone': {
            'offset': -14400,
        },
        'device': {
            'id': 'android-cb0d81d0e326cb42',
            'manufacturer': 'OnePlus',
            'model': 'ONEPLUS+A3010',
            'android_version': 25,
            'android_release': '7.1.1',
        }
    }
>>> threads = Threads(settings=settings, username=INSTAGRAM_USERNAME, password=INSTAGRAM_PASSWORD)
```

To provide settings as a path to a `JSON` file, use the following commands:

```python
>>> threads = Threads(
      settings='/Users/dmytrostriletskyi/projects/threads/settings.json',
      username='instagram_username',
      password='instagram_password',
  )
```

The `JSON` file content looks like:

```bash
$ cat settings.json
{
    "authentication": {
        "token": "eyJkc191c2VyX2lkIjYnNjA0MzgxNTQ1N1NjYiLCJzZXNzaW9uaWQiOi1FxSUQ1TXcifQ=="
    },
    "timezone": {
        "offset": -14400
    },
    "device": {
        "id": "android-2d53671ef3d1ac79",
        "manufacturer": "OnePlus",
        "model": "ONEPLUS+A3010",
        "android_version": 25,
        "android_release": "7.1.1"
    }
}
```

Instead of manually create a `JSON` file, you can ask `Threads` to generate it, specifying a path to where to save it, 
with the following command:

```python
>>> threads = Threads(username='instagram_username', password='instagram_password')
>>> threads.download_settings(path='/Users/dmytrostriletskyi/projects/threads/settings.json')
```

### Pagination

There are two things that are involved into the pagination for requests that respond with lists of records (e.g. threads, 
replies, followers, following and so on): `limit` and `max id`. Basically, `limit` is responsible for a number of records
to request (where `15` is a default number) and `max id` is kinda offset thingy, but a bit tricky.

Let consider the method for getting a list of a user's threads if there are only `10` in total:

```python
>>> user_threads = threads.private_api.get_user_threads(id=314216)
>>> user_threads
{
    "threads": [
        {"id": 3146710118003098789, ...},
        {"id": 3146710118003098788, ...},
        {"id": 3146710118003098787, ...},
        {"id": 3146710118003098786, ...},
        {"id": 3146710118003098785, ...},
        {"id": 3146710118003098784, ...},
        {"id": 3146710118003098783, ...},
        {"id": 3146710118003098782, ...},
        {"id": 3146710118003098781, ...},
        {"id": 3146710118003098780, ...},
    ],
    "status": "ok"
}
```

If we want to fetch them iteratively by chunks (for instance, by `2` threads), we just specify a `limit` argument:

```python
>>> user_threads = threads.private_api.get_user_threads(id=314216, limit=2)
>>> user_threads
{
    "threads": [
        {"id": 3146710118003098789, ...},  # 1st thread from 10.
        {"id": 3146710118003098788, ...},  # 2nd thread from 10.
    ],
    "next_max_id": "QVFCWnJRSFdwMW5ITjRhcDlqR2hfY19JM21eQ==",
    "status": "ok"
}
```

But it is not enough, because on this stage we do not know how to iterate and fetch the next `2` threads. Here `max id` 
helps, if you see, there is `next_max_id` in the response. Which is encoded identifier of the `3rd` thread to start 
offsetting (chunking) from:

```python
>>> user_threads = threads.private_api.get_user_threads(
        id=314216, 
        limit=2, 
        from_max_id='QVFCWnJRSFdwMW5ITjRhcDlqR2hfY19JM21eQ==',
    )
>>> user_threads
{
    "threads": [
        {"id": 3146710118003098787, ...},  # 3rd thread from 10.
        {"id": 3146710118003098786, ...},  # 4th thread from 10.
    ],
    "next_max_id": "ZrWDBSMm9DTU90Yy1mSXQ4WTNWb0V3ZmI4Um5==",
    "status": "ok"
}
```

So, basically, you should iterate over all threads until `next_max_id` is presented. The code to fetch all existing 
threads would look like:

```python
from_max_id = None

while True:
    user_threads = threads.private_api.get_user_threads(id=314216, limit=2, from_max_id=from_max_id)

    for thread in user_threads.get('threads'):
        items = thread.get('thread_items')

        for item in items:
            thread_id = item.get('post').get('pk')
            print(thread_id)

    from_max_id = user_threads.get('next_max_id')

    if from_max_id is None:
        break

3146710118003098789
3146710118003098788
3146710118003098787
3146710118003098786
3146710118003098785
3146710118003098784
3146710118003098783
3146710118003098782
3146710118003098781
3146710118003098780
```

Use the pagination if there are a lot of records (e.g. threads, replies, followers, following and so on) to be returned 
as well as if you are somehow limited by computer resources (`RAM`, `bandwidth` and so on).

### Public

#### User

##### Get Identifier

`threads.public_api.get_user_id` — getting a user's identifier by their username.

| Parameters |  Type   | Required | Restrictions | Description                                              |
|:----------:|:-------:|:--------:|:------------:|----------------------------------------------------------|
| `username` | String  |   Yes    |      -       | A user's username which goes along with `@` like `zuck`. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_id = threads.public_api.get_user_id(username='zuck')
  >>> user_id
  314216
  ```
</details>

##### Get By Identifier

`threads.public_api.get_user` — getting a user by their identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user = threads.public_api.get_user(id=314216)
  >>> user
  {
      "data": {
          "userData": {
              "user": {
                  "is_private": false,
                  "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX-mW2_l&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfAUZzobOIH6imLnb2Z3iXoWY5H1Fv_kNnyG8T4UGgJegQ&oe=64AED800&_nc_sid=10d13b",
                  "username": "zuck",
                  "hd_profile_pic_versions": [
                      {
                          "height": 320,
                          "url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s320x320&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX-mW2_l&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfD5z6UgnQH54dihPnMrXgH2L-mLCMGlFsIF9Ug7U4RWdA&oe=64AED800&_nc_sid=10d13b",
                          "width": 320
                      },
                      {
                          "height": 640,
                          "url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s640x640&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX-mW2_l&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfD4BaVu4cDcX53xPocD-3o_ZbKIESxUZhlU08FBpycCsA&oe=64AED800&_nc_sid=10d13b",
                          "width": 640
                      }
                  ],
                  "is_verified": true,
                  "biography": "",
                  "biography_with_entities": null,
                  "follower_count": 2663947,
                  "profile_context_facepile_users": null,
                  "bio_links": [
                      {
                          "url": ""
                      }
                  ],
                  "pk": "314216",
                  "full_name": "Mark Zuckerberg",
                  "id": null
              }
          }
      },
      "extensions": {
          "is_final": true
      }
  }
  ```
</details>

##### Get Threads

`threads.public_api.get_user_threads` — getting a user's threads by the user's identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_threads = threads.public_api.get_user_threads(id=314216)
  >>> user_threads
  {
      "instagram": {
          "pk": "314216",
          "username": "zuck",
          "full_name": "Mark Zuckerberg",
          "is_private": false,
          "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/352224138_1028122805231303_1175896139426286760_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=hbekpcjRfioAX8iYGJv&edm=AKEQFekBAAAA&ccb=7-5&oh=00_AfDf2r6qwujUc84tkzUlYJMfJt66xoWScQ-nsB5bmtYDnw&oe=64B0066A&_nc_sid=29ddf3",
          "profile_pic_url_hd": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/352224138_1028122805231303_1175896139426286760_n.jpg?stp=dst-jpg_s320x320&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=hbekpcjRfioAX8iYGJv&edm=AKEQFekBAAAA&ccb=7-5&oh=00_AfCoPcYmzHdc2evDvduZjZK-IxDS07wGf0x9czecY_TgVQ&oe=64B0066A&_nc_sid=29ddf3",
          "is_verified": true,
          "media_count": 283,
          "follower_count": 11924434,
          "following_count": 523,
          "biography": "",
          "external_url": null,
          "account_type": null,
          "is_business": false,
          "public_email": null,
          "contact_phone_number": null,
          "public_phone_country_code": null,
          "public_phone_number": null,
          "business_contact_method": "UNKNOWN",
          "business_category_name": null,
          "category_name": "Entrepreneur",
          "category": null,
          "address_street": null,
          "city_id": null,
          "city_name": null,
          "latitude": null,
          "longitude": null,
          "zip": null,
          "instagram_location_id": null,
          "interop_messaging_user_fbid": null
      },
      "threads": {
          "data": {
              "userData": {
                  "user": {
                      "is_private": false,
                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX9JAAZI&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfCdAMkmk0XL_r0GQi2MRD1Aq1kPZKBLfXLby47e_hsZrg&oe=64AED800&_nc_sid=10d13b",
                      "username": "zuck",
                      "hd_profile_pic_versions": [
                          {
                              "height": 320,
                              "url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s320x320&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX9JAAZI&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfDJeE127_ZFA-eD3qRMM0Fh2NM-jRR4tUFsTywCrMctNA&oe=64AED800&_nc_sid=10d13b",
                              "width": 320
                          },
                          {
                              "height": 640,
                              "url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s640x640&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX9JAAZI&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfAY-75SdPDasc0ophRu3lgeeHmnb3qPZIE-mCPh8PRRBw&oe=64AED800&_nc_sid=10d13b",
                              "width": 640
                          }
                      ],
                      "is_verified": true,
                      "biography": "",
                      "biography_with_entities": null,
                      "follower_count": 2663588,
                      "profile_context_facepile_users": null,
                      "bio_links": [
                          {
                              "url": ""
                          }
                      ],
                      "pk": "314216",
                      "full_name": "Mark Zuckerberg",
                      "id": null
                  }
              }
          },
          "extensions": {
              "is_final": true
          }
      }
  }
  ```
</details>

##### Get Replies

`threads.public_api.get_user_replies` — getting a user's replies by the user's identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_replies = threads.public_api.get_user_replies(id=314216)
  >>> user_replies
  {
      "data": {
          "mediaData": {
              "threads": [
                  {
                      "thread_items": [
                          {
                              "post": {
                                  "user": {
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358196917_1319101762021874_6840458701147612733_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=RYUCB0OzBK8AX-fwWgu&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfAyNgzr1QLbjIdorzFD01h-NpApHx7-0XKdGWOKjS4Xlw&oe=64ABF5FF&_nc_sid=10d13b",
                                      "username": "intense0ne",
                                      "id": null,
                                      "is_verified": true,
                                      "pk": "13455834"
                                  },
                                  "image_versions2": {
                                      "candidates": [
                                          {
                                              "height": 1431,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfCUsy68lK2H5iyEGnWnPrQMnd18Dpvl0ZOhGh-iWQpxZA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 1170,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 881,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35_p720x720&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfBUyEmnvY6wewYd2xR5RcBUdpwYDQ3-tPOJroowQwDDZA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 720,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 783,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35_p640x640_sh0.08&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfCngfqkXR6lMDsTO_3W3ub2wXo6gy8hs9IflxKgVSZ29Q&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 640,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 587,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35_p480x480&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfDEcZTEK5c-C1M5RO0NLJZ1afo7GGqHClM6SFBH6nVJqQ&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 480,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 391,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35_p320x320&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfDme7hPMR0v7Uc_Rq5Vl-HA3eUAosNOuznntJ3rwkPVgA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 320,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 294,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=dst-jpg_e35_p240x240&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfDKjwryrv-cxfybvuPaa9TbcBqxozsTeOU6I7fRgDaZVw&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 240,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 1080,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s1080x1080&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfAfUg9I6LBwwruVJxP0y7tsa1eILSH3lnERy-GhLvY5tQ&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 1080,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 750,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s750x750_sh0.08&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfC4eDvJfpOFQXP9iDiQccDFa8mH-c-4eHjtoJa1guTIFA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 750,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 640,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s640x640_sh0.08&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfCz0RbvKHBOKMTsmaHFJTa2NUymRFh9abONGDVhps-3BQ&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 640,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 480,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s480x480&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfCp9r6Se5hjBo-rMHJvLhfDFRJkzbOo7QZu-BkiW-kCew&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 480,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 320,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s320x320&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfCizo3gIxHuMKtgDmK1XNlxXr2NyEwfRbsN8ZLNb6y8GA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 320,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 240,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s240x240&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfDvAFSgCbR982_ZAUAFSfdyXLWG_7WbXnv-_UZnorY7EA&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 240,
                                              "__typename": "XDTImageCandidate"
                                          },
                                          {
                                              "height": 150,
                                              "url": "https://scontent.cdninstagram.com/v/t51.2885-15/358024814_953839475848219_4275939482568010016_n.jpg?stp=c0.130.1170.1170a_dst-jpg_e35_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=XBVzqlYSSPMAX-rTJuQ&edm=APs17CUBAAAA&ccb=7-5&ig_cache_key=MzE0MTMwNzMyODM1ODU1NjA4OA%3D%3D.2-ccb7-5&oh=00_AfAy_iWsQikPBVk0haExvRIaDHqtPRkh2t-A9DHE4G2HWg&oe=64ACB5E4&_nc_sid=10d13b",
                                              "width": 150,
                                              "__typename": "XDTImageCandidate"
                                          }
                                      ]
                                  },
                                  "original_width": 1170,
                                  "original_height": 1431,
                                  "video_versions": [],
                                  "carousel_media": null,
                                  "carousel_media_count": null,
                                  "pk": "3141307328358556088",
                                  "has_audio": null,
                                  "text_post_app_info": {
                                      "link_preview_attachment": null,
                                      "share_info": {
                                          "quoted_post": null,
                                          "reposted_post": null
                                      },
                                      "reply_to_author": null,
                                      "is_post_unavailable": false
                                  },
                                  "caption": {
                                      "text": "Volk or Yair?? #UFC290\n\nI got Volk!! \ud83d\udc4f\ud83d\udc4f\ud83d\udc4f"
                                  },
                                  "taken_at": 1688693036,
                                  "like_count": 4144,
                                  "code": "CuYKl8tJKG4",
                                  "media_overlay_info": null,
                                  "id": "3141307328358556088_13455834"
                              },
                              "line_type": "squiggle",
                              "view_replies_cta_string": "386 replies",
                              "reply_facepile_users": [
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358513246_834402004938078_3745574617102161833_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=106&_nc_ohc=LtUMjbsa1lgAX83w0Sh&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfDGvGlfVhbbAHunncfE-Gzmbf0rshNG8tlfg4eb5V77iQ&oe=64AD69D4&_nc_sid=10d13b"
                                  },
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358028941_1385051345389996_8437393621281150341_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=104&_nc_ohc=XcGqYzbKhDYAX87Clm7&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfCKDgkhOitXrGIkYQs1V607cQ9rw0cSB9KuZoJTyCIPWQ&oe=64ABFFDC&_nc_sid=10d13b"
                                  },
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358164004_986964552348931_381538637050666409_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=ML6ZsvS6xFsAX_GLu5T&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfBjZQ0RElFZ0DQxOpHmDIw-p2WRX9_NvhArpVy8Fqz0lA&oe=64AC0D96&_nc_sid=10d13b"
                                  }
                              ],
                              "should_show_replies_cta": true,
                              "__typename": "XDTThreadItem"
                          },
                          {
                              "post": {
                                  "user": {
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=9PG1NK-L8OkAX_mdst3&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfAo5WPQy4q_RbWq8Hox7lQS09zUmk8PHdJWXYjU8Tu2yw&oe=64ACDDC0&_nc_sid=10d13b",
                                      "username": "zuck",
                                      "id": null,
                                      "is_verified": true,
                                      "pk": "314216"
                                  },
                                  "image_versions2": {
                                      "candidates": []
                                  },
                                  "original_width": 612,
                                  "original_height": 612,
                                  "video_versions": [],
                                  "carousel_media": null,
                                  "carousel_media_count": null,
                                  "pk": "3141314003249945904",
                                  "has_audio": null,
                                  "text_post_app_info": {
                                      "link_preview_attachment": null,
                                      "share_info": {
                                          "quoted_post": null,
                                          "reposted_post": null
                                      },
                                      "reply_to_author": {
                                          "username": "intense0ne",
                                          "id": null
                                      },
                                      "is_post_unavailable": false
                                  },
                                  "caption": {
                                      "text": "Definitely Volk"
                                  },
                                  "taken_at": 1688693832,
                                  "like_count": 10843,
                                  "code": "CuYMHFLrF0w",
                                  "media_overlay_info": null,
                                  "id": "3141314003249945904_314216"
                              },
                              "line_type": "line",
                              "view_replies_cta_string": "1,114 replies",
                              "reply_facepile_users": [
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/357993149_654182843249841_2984126770891448042_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=109&_nc_ohc=-HCIQXYHQlcAX8qkTXo&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfAwsxFT9ZYe71DKt7Wm3tmH4-3SwpA_6ifb74G-bMn8Bg&oe=64AC46A1&_nc_sid=10d13b"
                                  },
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358036814_264065539563777_3872673087942117120_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=109&_nc_ohc=T3x8fyZcDQIAX_FXQ-6&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfDtWBscXjH85fml9eI9IERX2wibGkWU-EQcuSH-b3MvCA&oe=64AD33EE&_nc_sid=10d13b"
                                  },
                                  {
                                      "__typename": "XDTUserDict",
                                      "id": null,
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/358000233_1044681130274316_6742390706661437266_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=100&_nc_ohc=PvpO-62JaD0AX_XMP5_&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfC4ma-opT9ieAnypxLSXNtVUOQkSq5_hbtca5ADqOSHRA&oe=64ACA63A&_nc_sid=10d13b"
                                  }
                              ],
                              "should_show_replies_cta": true,
                              "__typename": "XDTThreadItem"
                          }
                      ],
                      "id": "3141314003249945904"
                  },
                  ...
              ]
          }
      },
      "extensions": {
          "is_final": true
      }
  }
  ```
</details>

#### Thread

##### Get Identifier

`threads.public_api.get_thread_id` — getting a thread's identifier by its `URL` identifier. `URL` identifier is a last 
part of a thread's website `URL`. If the thread's `URL` is `https://threads.net/t/CuXFPIeLLod`, then it would be `CuXFPIeLLod`.

| Parameters |  Type  | Required | Restrictions | Description                  |
|:----------:|:------:|:--------:|:------------:|------------------------------|
|  `url_id`  | String |   Yes    |      -       | A thread's `URL` identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread_id = threads.public_api.get_thread_id(url_id='CuXFPIeLLod')
  >>> thread_id
  3141002295235099165
  ```
</details>

##### Get By Identifier

`threads.public_api.get_thread` — getting a thread by its identifier.

| Parameters |  Type   | Required | Restrictions | Description            |
|:----------:|:-------:|:--------:|:------------:|------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A thread's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread = threads.public_api.get_thread(id=3141002295235099165)
  >>> thread
  {
      "data": {
          "data": {
              "containing_thread": {
                  "thread_items": [
                      {
                          "post": {
                              "user": {
                                  "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/343392897_618515990300243_8088199406170073086_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=1xtR491RY6cAX8Okf3Z&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfDqsZTCRQlz7EaDpgEzBkmXqtqKtTH80Q0r1rIDMJCcpg&oe=64AC2D8D&_nc_sid=10d13b",
                                  "username": "mosseri",
                                  "id": null,
                                  "is_verified": true,
                                  "pk": "95561"
                              },
                              "image_versions2": {
                                  "candidates": []
                              },
                              "original_width": 612,
                              "original_height": 612,
                              "video_versions": [],
                              "carousel_media": null,
                              "carousel_media_count": null,
                              "pk": "3141055616164096839",
                              "has_audio": null,
                              "text_post_app_info": {
                                  "link_preview_attachment": null,
                                  "share_info": {
                                      "quoted_post": null,
                                      "reposted_post": null
                                  },
                                  "reply_to_author": null,
                                  "is_post_unavailable": false,
                                  "direct_reply_count": 2839
                              },
                              "caption": {
                                  "text": "I've been getting some questions about deleting your account. To clarify, you can deactivate your Threads account, which hides your Threads profile and content, you can set your profile to private, and you can delete individual threads posts \u2013 all without deleting your Instagram account. Threads is powered by Instagram, so right now it's just one account, but we're looking into a way to delete your Threads account separately."
                              },
                              "taken_at": 1688663030,
                              "like_count": 29984,
                              "code": "CuXRXDdNOtH",
                              "media_overlay_info": null,
                              "id": "3141055616164096839_95561"
                          },
                          "line_type": "none",
                          "view_replies_cta_string": "2,839 replies",
                          "reply_facepile_users": [],
                          "should_show_replies_cta": true
                      }
                  ],
                  "thread_type": "thread",
                  "header": null,
                  "id": "3141055616164096839"
              },
              "reply_threads": [
                  {
                      "thread_items": [
                          {
                              "post": {
                                  "user": {
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/355424589_948742193018772_4617955233166333765_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=100&_nc_ohc=heQWdfsKG5MAX9mLim0&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfA6TUpLjjegnP6KuducRm8n6uU9iBCXg4O1P33WYSh3mg&oe=64ACDDBA&_nc_sid=10d13b",
                                      "username": "jimmywhotalks",
                                      "id": null,
                                      "is_verified": true,
                                      "pk": "51094265817"
                                  },
                                  "image_versions2": {
                                      "candidates": []
                                  },
                                  "original_width": 612,
                                  "original_height": 612,
                                  "video_versions": [],
                                  "carousel_media": null,
                                  "carousel_media_count": null,
                                  "pk": "3141082664316809193",
                                  "has_audio": null,
                                  "text_post_app_info": {
                                      "link_preview_attachment": null,
                                      "share_info": {
                                          "quoted_post": null,
                                          "reposted_post": null
                                      },
                                      "reply_to_author": {
                                          "username": "mosseri",
                                          "id": null
                                      },
                                      "is_post_unavailable": false
                                  },
                                  "caption": {
                                      "text": "Glad to know, Everyone is worrying for that."
                                  },
                                  "taken_at": 1688666254,
                                  "like_count": 59,
                                  "code": "CuXXgqAva_p",
                                  "media_overlay_info": null,
                                  "id": "3141082664316809193_51094265817"
                              },
                              "line_type": "none",
                              "view_replies_cta_string": null,
                              "reply_facepile_users": [],
                              "should_show_replies_cta": false
                          }
                      ],
                      "thread_type": "thread",
                      "header": null,
                      "id": "3141082664316809193"
                  },
                  ...
              ]
          }
      },
      "extensions": {
          "is_final": true
      }
  }
  ```
</details>

##### Get Likers

`threads.public_api.get_thread_likers` — getting a thread's likers by the thread's identifier.

| Parameters |  Type   | Required | Restrictions | Description            |
|:----------:|:-------:|:--------:|:------------:|------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A thread's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread_likers = threads.public_api.get_thread_likers(id=3141002295235099165)
  >>> thread_likers
  {
      "data": {
          "likers": {
              "users": [
                  {
                      "pk": "32729880576",
                      "full_name": "K 🦋I🦋K🦋I",
                      "profile_pic_url": "https://scontent.cdninstagram.com/19/6007.jpg
                      "follower_count": 51,
                      "is_verified": false,
                      "username": "niyod_couture",
                      "profile_context_facepile_users": null,
                      "id": null
                  },
                  ...
              ]
          }
      },
      "extensions": {
          "is_final": true
      }
  }
  ```
</details>

### Private

#### User

##### Get Identifier

`threads.private_api.get_user_id` — getting a user's identifier by their username.

| Parameters |  Type   | Required | Restrictions | Description                                              |
|:----------:|:-------:|:--------:|:------------:|----------------------------------------------------------|
| `username` | String  |   Yes    |      -       | A user's username which goes along with `@` like `zuck`. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_id = threads.private_api.get_user_id(username='zuck')
  >>> user_id
  314216
  ```
</details>

##### Get By Identifier

`threads.private_api.get_user` — getting a user by their identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user = threads.private_api.get_user(id=314216)
  >>> user
  {
      "user": {
          "has_anonymous_profile_picture": false,
          "is_supervision_features_enabled": false,
          "follower_count": 2782815,
          "media_count": 283,
          "following_count": 313,
          "following_tag_count": 3,
          "can_use_affiliate_partnership_messaging_as_creator": false,
          "can_use_affiliate_partnership_messaging_as_brand": false,
          "has_collab_collections": false,
          "has_private_collections": true,
          "has_music_on_profile": false,
          "is_potential_business": false,
          "can_use_branded_content_discovery_as_creator": false,
          "can_use_branded_content_discovery_as_brand": false,
          "fan_club_info": {
              "fan_club_id": null,
              "fan_club_name": null,
              "is_fan_club_referral_eligible": null,
              "fan_consideration_page_revamp_eligiblity": null,
              "is_fan_club_gifting_eligible": null,
              "subscriber_count": null,
              "connected_member_count": null,
              "autosave_to_exclusive_highlight": null,
              "has_enough_subscribers_for_ssc": null
          },
          "fbid_v2": "17841401746480004",
          "pronouns": [],
          "is_whatsapp_linked": false,
          "transparency_product_enabled": false,
          "account_category": "",
          "interop_messaging_user_fbid": 119171612803872,
          "bio_links": [
              {
                  "link_id": 17979756671109835,
                  "url": "",
                  "lynx_url": "",
                  "link_type": "external",
                  "title": "",
                  "open_external_url_with_in_app_browser": true
              }
          ],
          "can_add_fb_group_link_on_profile": false,
          "external_url": "",
          "show_shoppable_feed": false,
          "merchant_checkout_style": "none",
          "seller_shoppable_feed_type": "none",
          "creator_shopping_info": {
              "linked_merchant_accounts": []
          },
          "has_guides": false,
          "has_highlight_reels": false,
          "hd_profile_pic_url_info": {
              "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX_8Gm2Q&edm=AEF8tYYBAAAA&ccb=7-5&oh=00_AfBs65PPEgj6s58aEAVAZWAUJu9JqhMcsWV8VLubRyzQwQ&oe=64B0D240&_nc_sid=1e20d2",
              "width": 805,
              "height": 805
          },
          "hd_profile_pic_versions": [
              {
                  "width": 320,
                  "height": 320,
                  "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s320x320&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX_8Gm2Q&edm=AEF8tYYBAAAA&ccb=7-5&oh=00_AfB7HZGMI4QLDwO-wKkntDBbGavE_-oKHus264KgonCVqg&oe=64B0D240&_nc_sid=1e20d2"
              },
              {
                  "width": 640,
                  "height": 640,
                  "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s640x640&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX_8Gm2Q&edm=AEF8tYYBAAAA&ccb=7-5&oh=00_AfBxpkJK3xNM4BLNfbMzrcTyKkza60pitJvggFKtuX4uIA&oe=64B0D240&_nc_sid=1e20d2"
              }
          ],
          "is_interest_account": true,
          "is_favorite": false,
          "is_favorite_for_stories": false,
          "is_favorite_for_igtv": false,
          "is_favorite_for_clips": false,
          "is_favorite_for_highlights": false,
          "broadcast_chat_preference_status": {
              "json_response": "{\"status\":\"ok\",\"status_code\":\"200\",\"is_broadcast_chat_creator\":true,\"notification_setting_type\":2}"
          },
          "live_subscription_status": "default",
          "usertags_count": 43060,
          "total_ar_effects": 1,
          "total_clips_count": 1,
          "has_videos": true,
          "total_igtv_videos": 12,
          "has_igtv_series": false,
          "biography": "",
          "include_direct_blacklist_status": true,
          "biography_with_entities": {
              "raw_text": "",
              "entities": []
          },
          "show_fb_link_on_profile": false,
          "primary_profile_link_type": 0,
          "can_hide_category": true,
          "can_hide_public_contacts": true,
          "should_show_category": false,
          "category_id": 1617,
          "is_category_tappable": false,
          "should_show_public_contacts": false,
          "is_eligible_for_smb_support_flow": true,
          "is_eligible_for_lead_center": false,
          "is_experienced_advertiser": false,
          "lead_details_app_id": "com.bloks.www.ig.smb.lead_gen.subpage",
          "is_business": false,
          "professional_conversion_suggested_account_type": 3,
          "account_type": 3,
          "direct_messaging": "",
          "instagram_location_id": "",
          "address_street": "",
          "business_contact_method": "UNKNOWN",
          "city_id": 0,
          "city_name": "",
          "contact_phone_number": "",
          "is_profile_audio_call_enabled": false,
          "latitude": 0.0,
          "longitude": 0.0,
          "public_email": "",
          "public_phone_country_code": "",
          "public_phone_number": "",
          "zip": "",
          "mutual_followers_count": 0,
          "has_onboarded_to_text_post_app": true,
          "show_text_post_app_badge": true,
          "text_post_app_joiner_number": 1,
          "show_ig_app_switcher_badge": true,
          "show_text_post_app_switcher_badge": true,
          "profile_context": "",
          "profile_context_links_with_user_ids": [],
          "profile_context_facepile_users": [],
          "has_chaining": true,
          "pk": 314216,
          "pk_id": "314216",
          "username": "zuck",
          "full_name": "Mark Zuckerberg",
          "is_private": false,
          "follow_friction_type": 0,
          "is_verified": true,
          "profile_pic_id": "3138909791791822006_314216",
          "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX_8Gm2Q&edm=AEF8tYYBAAAA&ccb=7-5&oh=00_AfD5zX6BkQpgR-H-iTsZ2PlwDEOXnf7PCbohypiV8Aj10g&oe=64B0D240&_nc_sid=1e20d2",
          "current_catalog_id": null,
          "mini_shop_seller_onboarding_status": null,
          "shopping_post_onboard_nux_type": null,
          "ads_incentive_expiration_date": null,
          "displayed_action_button_partner": null,
          "smb_delivery_partner": null,
          "smb_support_delivery_partner": null,
          "displayed_action_button_type": "",
          "smb_support_partner": null,
          "is_call_to_action_enabled": false,
          "num_of_admined_pages": null,
          "category": "Entrepreneur",
          "account_badges": [],
          "show_account_transparency_details": true,
          "existing_user_age_collection_enabled": true,
          "show_post_insights_entry_point": true,
          "has_public_tab_threads": true,
          "third_party_downloads_enabled": 1,
          "is_regulated_c18": false,
          "is_in_canada": false,
          "profile_type": 0,
          "is_profile_broadcast_sharing_enabled": true,
          "has_exclusive_feed_content": false,
          "has_fan_club_subscriptions": false,
          "is_memorialized": false,
          "open_external_url_with_in_app_browser": true,
          "pinned_channels_info": {
              "pinned_channels_list": [],
              "has_public_channels": false
          },
          "request_contact_enabled": false,
          "robi_feedback_source": null,
          "chaining_results": null,
          "is_bestie": false,
          "remove_message_entrypoint": false,
          "auto_expand_chaining": false,
          "is_new_to_instagram": false,
          "highlight_reshare_disabled": false
      },
      "status": "ok"
  }
  ```
</details>

##### Get Threads

`threads.private_api.get_user_thread` — getting a user's threads by the user's identifier. Supports 
[pagination](#pagination).

|  Parameters   |  Type   | Required | Restrictions | Description                                              |
|:-------------:|:-------:|:--------:|:------------:|----------------------------------------------------------|
|     `id`      | Integer |   Yes    |     `>0`     | A user's identifier.                                     |
|    `limit`    | Integer |    No    |     `>0`     | A number of threads to get. Default value is `15`.       |
| `from_max_id` | String  |    No    |      -       | An encoded thread's identifier to start offsetting from. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_threads = threads.private_api.get_user_threads(id=314216)
  >>> user_threads
  {
      "medias": [],
      "threads": [
          {
              "thread_items": [
                  {
                      "post": {
                          "pk": 3143857175171939857,
                          "id": "3143857175171939857_314216",
                          "taken_at": 1688997002,
                          "device_timestamp": 154381465597879,
                          "client_cache_key": "MzE0Mzg1NzE3NTE3MTkzOTg1Nw==.2",
                          "filter_type": 0,
                          "like_and_view_counts_disabled": false,
                          "integrity_review_decision": "pending",
                          "text_post_app_info": {
                              "is_post_unavailable": false,
                              "is_reply": false,
                              "reply_to_author": null,
                              "direct_reply_count": 24278,
                              "self_thread_count": 0,
                              "reply_facepile_users": [],
                              "link_preview_attachment": null,
                              "can_reply": true,
                              "reply_control": "everyone",
                              "hush_info": null,
                              "share_info": {
                                  "can_repost": true,
                                  "is_reposted_by_viewer": false,
                                  "can_quote_post": true
                              }
                          },
                          "caption": {
                              "pk": "17971080284299711",
                              "user_id": 314216,
                              "text": "Threads reached 100 million sign ups over the weekend. That's mostly organic demand and we haven't even turned on many promotions yet. Can't believe it's only been 5 days!",
                              "type": 1,
                              "created_at": 1688997002,
                              "created_at_utc": 1688997002,
                              "content_type": "comment",
                              "status": "Active",
                              "bit_flags": 0,
                              "did_report_as_spam": false,
                              "share_enabled": false,
                              "user": {
                                  "pk": 314216,
                                  "pk_id": "314216",
                                  "username": "zuck",
                                  "full_name": "Mark Zuckerberg",
                                  "is_private": false,
                                  "is_verified": true,
                                  "profile_pic_id": "3138909791791822006_314216",
                                  "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=XKEMOZTrwUAAX-1hCyT&edm=AKIuHiwBAAAA&ccb=7-5&oh=00_AfCXFLPZTQxrVDWADA_aPG8o4_lRhKbEQsAs6Jh5TF3auQ&oe=64B6C100&_nc_sid=3b7ee3",
                                  "fbid_v2": "17841401746480004",
                                  "has_onboarded_to_text_post_app": true
                              },
                              "is_covered": false,
                              "is_ranked_comment": false,
                              "media_id": 3143857175171939857,
                              "private_reply_status": 0
                          },
                          "media_type": 19,
                          "code": "CuhOXGmr74R",
                          "product_type": "text_post",
                          "organic_tracking_token": "eyJ2ZXJzaW9uIjo1LCJwYXlsb2FkIjp7ImlzX2FuYWx5dGljc190cmFja2VkIjp0cnVlLCJ1dWlkIjoiYzY0NzE2ZDZmYmJhNDE2MDlhODEwMWU3ZTkzYmM3NWYzMTQzODU3MTc1MTcxOTM5ODU3Iiwic2VydmVyX3Rva2VuIjoiMTY4OTQxOTI0NDgxMHwzMTQzODU3MTc1MTcxOTM5ODU3fDYwNDM4MTU0NTY2fGFiYzJhZWFjNGZkZTBlZTFlN2I4ZTZhNTA4MDcxMzQxNGZjM2U0NTdmN2ZjZmYyZDE4ODlkYmQyODViOTBiZjYifSwic2lnbmF0dXJlIjoiIn0=",
                          "image_versions2": {
                              "candidates": []
                          },
                          "original_width": 612,
                          "original_height": 612,
                          "video_versions": [],
                          "like_count": 127327,
                          "has_liked": false,
                          "can_viewer_reshare": true,
                          "top_likers": [],
                          "user": {
                              "pk": 314216,
                              "pk_id": "314216",
                              "username": "zuck",
                              "full_name": "Mark Zuckerberg",
                              "is_private": false,
                              "is_verified": true,
                              "profile_pic_id": "3138909791791822006_314216",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=XKEMOZTrwUAAX-1hCyT&edm=AKIuHiwBAAAA&ccb=7-5&oh=00_AfCXFLPZTQxrVDWADA_aPG8o4_lRhKbEQsAs6Jh5TF3auQ&oe=64B6C100&_nc_sid=3b7ee3",
                              "friendship_status": {
                                  "following": false,
                                  "followed_by": false,
                                  "blocking": false,
                                  "muting": false,
                                  "is_private": false,
                                  "incoming_request": false,
                                  "outgoing_request": false,
                                  "text_post_app_pre_following": false,
                                  "is_bestie": false,
                                  "is_restricted": false,
                                  "is_feed_favorite": false,
                                  "is_eligible_to_subscribe": false
                              },
                              "has_anonymous_profile_picture": false,
                              "has_onboarded_to_text_post_app": true,
                              "account_badges": []
                          }
                      },
                      "line_type": "line",
                      "view_replies_cta_string": "24,278 replies",
                      "should_show_replies_cta": true,
                      "reply_facepile_users": [
                          {
                              "pk": 57325599231,
                              "pk_id": "57325599231",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/359520387_1561960634615463_4231364575815896983_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=103&_nc_ohc=L0Vg4L6V5_AAX_Hbcgj&edm=AKIuHiwBAAAA&ccb=7-5&oh=00_AfD130-XjZ1RrsD_TVSbyH1SgizO5hGb5W5IJhVTOK3_Gg&oe=64B6B3CC&_nc_sid=3b7ee3"
                          },
                          {
                              "pk": 53041190925,
                              "pk_id": "53041190925",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/359187805_177094641886545_7812186447575399344_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=109&_nc_ohc=0CBUtKwuehgAX9EK7C3&edm=AKIuHiwBAAAA&ccb=7-5&oh=00_AfDT8r2ogdVP6qGgBbsjZEoB9tL3V5Zyjwe-rWgUqjRDfw&oe=64B6AA34&_nc_sid=3b7ee3"
                          },
                          {
                              "pk": 60184849158,
                              "pk_id": "60184849158",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358782577_620441826734229_2771436956212880281_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=109&_nc_ohc=UwO4_ipDCRgAX-LM3Qn&edm=AKIuHiwBAAAA&ccb=7-5&oh=00_AfAie1X0UbKUYRbBtRI8fn_8_sbyZAy9sLKQjdTBiWhhNQ&oe=64B6B8B8&_nc_sid=3b7ee3"
                          }
                      ],
                      "can_inline_expand_below": false
                  }
              ],
              "thread_type": "thread",
              "show_create_reply_cta": false,
              "id": 3143857175171939857,
              ...
          },
          ...
      ],
      "status": "ok"
  }
  ```
</details>

##### Get Replies

`threads.private_api.get_user_replies — getting a user's replies by the user's identifier. Supports 
[pagination](#pagination).

|  Parameters   |  Type   | Required | Restrictions | Description                                             |
|:-------------:|:-------:|:--------:|:------------:|---------------------------------------------------------|
|     `id`      | Integer |   Yes    |     `>0`     | A user's identifier.                                    |
|    `limit`    | Integer |    No    |     `>0`     | A number of replies to get. Default value is `15`.      |
| `from_max_id` | String  |    No    |      -       | An encoded reply's identifier to start offsetting from. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_replies = threads.private_api.get_user_replies(id=314216)
  >>> user_replies
  {
      "medias": [],
      "threads": [
          {
              "thread_items": [
                  {
                      "post": {
                          "pk": 3146315493980433758,
                          "id": "3146315493980433758_9155181033",
                          "taken_at": 1689290074,
                          "device_timestamp": 1689290056390615,
                          "client_cache_key": "MzE0NjMxNTQ5Mzk4MDQzMzc1OA==.2",
                          "filter_type": 0,
                          "like_and_view_counts_disabled": false,
                          "integrity_review_decision": "pending",
                          "text_post_app_info": {
                              "is_post_unavailable": false,
                              "is_reply": false,
                              "reply_to_author": null,
                              "direct_reply_count": 348,
                              "self_thread_count": 0,
                              "reply_facepile_users": [],
                              "link_preview_attachment": null,
                              "can_reply": true,
                              "reply_control": "everyone",
                              "hush_info": null,
                              "share_info": {
                                  "can_repost": true,
                                  "is_reposted_by_viewer": false,
                                  "can_quote_post": true
                              }
                          },
                          "caption": {
                              "pk": "17869797770898727",
                              "user_id": 9155181033,
                              "text": "Squad goals \ud83d\ude0e\n\n(via @stylebender)",
                              "type": 1,
                              "created_at": 1689290074,
                              "created_at_utc": 1689290074,
                              "content_type": "comment",
                              "status": "Active",
                              "bit_flags": 0,
                              "did_report_as_spam": false,
                              "share_enabled": false,
                              "user": {
                                  "pk": 9155181033,
                                  "pk_id": "9155181033",
                                  "username": "espnmma",
                                  "full_name": "ESPN MMA",
                                  "is_private": false,
                                  "is_verified": true,
                                  "profile_pic_id": "3141004987567665802_9155181033",
                                  "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358383703_848206763406082_8252942189546556167_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=110&_nc_ohc=WOV8wy4ScRoAX_C1CaW&edm=AClQE0sBAAAA&ccb=7-5&oh=00_AfDDl35PMIh3bUJ2WuzOkIIAgIEZqwFqdSh8tuPDTyfgTg&oe=64B7C4EE&_nc_sid=2bf346",
                                  "fbid_v2": "17841409270994397",
                                  "has_onboarded_to_text_post_app": true
                              },
                              "is_covered": false,
                              "is_ranked_comment": false,
                              "media_id": 3146315493980433758,
                              "private_reply_status": 0
                          },
                          "media_type": 2,
                          "code": "Cup9UWaAu1e",
                          "product_type": "feed",
                          "original_width": 640,
                          "original_height": 1136,
                          "is_dash_eligible": 1,
                          "video_codec": "avc1.64001e",
                          "has_audio": true,
                          "video_duration": 18.784,
                          "video_versions": [
                              {
                                  "type": 101,
                                  "width": 720,
                                  "height": 1280,
                                  "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t50.2886-16/360404858_562471725885766_7209197166491385251_n.mp4?efg=eyJ2ZW5jb2RlX3RhZyI6InZ0c192b2RfdXJsZ2VuLjcyMC5mZWVkLmJhc2VsaW5lIn0&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=110&_nc_ohc=m3bTlD7JhCoAX9eODcJ&edm=AClQE0sBAAAA&vs=2611672838971644_3296775565&_nc_vs=HBksFQAYJEdIcFhleFZHNldxbmtQOEJBS085ZUlDZ05neGtia1lMQUFBRhUAAsgBABUAGCRHTm1ZaFJXano5Rk1UM2dBQU0wN3I2d1lrVng2YmtZTEFBQUYVAgLIAQAoABgAGwGIB3VzZV9vaWwBMBUAACa6u7r%2BmOjoPxUCKAJDMywXQDKzMzMzMzMYEmRhc2hfYmFzZWxpbmVfMV92MREAdeoHAA%3D%3D&_nc_rid=e91b3f00fc&ccb=7-5&oh=00_AfB17yCrQdXrNLAZlgUqwZSSjgrwF9H8oNjJF6eVpHx9Ug&oe=64B477DB&_nc_sid=2bf346",
                                  "id": "2611672838971644v"
                              },
                              {
                                  "type": 102,
                                  "width": 480,
                                  "height": 854,
                                  "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t50.2886-16/361062149_663346358581709_8844241500491019989_n.mp4?efg=eyJ2ZW5jb2RlX3RhZyI6InZ0c192b2RfdXJsZ2VuLjQ4MC5mZWVkLmJhc2VsaW5lIn0&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=105&_nc_ohc=V4Gn1XoC7usAX8ESNnG&edm=AClQE0sBAAAA&vs=1023383492024442_1730708493&_nc_vs=HBksFQAYJEdBVmZoUlhOX1JCY1Qxc0NBTlgyaURESkRyMTZia1lMQUFBRhUAAsgBABUAGCRHTm1ZaFJXano5Rk1UM2dBQU0wN3I2d1lrVng2YmtZTEFBQUYVAgLIAQAoABgAGwGIB3VzZV9vaWwBMBUAACa6u7r%2BmOjoPxUCKAJDMywXQDKzMzMzMzMYEmRhc2hfYmFzZWxpbmVfMl92MREAdeoHAA%3D%3D&_nc_rid=e91b3f00fc&ccb=7-5&oh=00_AfD4G2wv4eJTNJjnGXOF04FdWuUUtelx_tvm5iq5dYx1pw&oe=64B49F3C&_nc_sid=2bf346",
                                  "id": "1023383492024442v"
                              },
                              {
                                  "type": 103,
                                  "width": 480,
                                  "height": 854,
                                  "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t50.2886-16/361062149_663346358581709_8844241500491019989_n.mp4?efg=eyJ2ZW5jb2RlX3RhZyI6InZ0c192b2RfdXJsZ2VuLjQ4MC5mZWVkLmJhc2VsaW5lIn0&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=105&_nc_ohc=V4Gn1XoC7usAX8ESNnG&edm=AClQE0sBAAAA&vs=1023383492024442_1730708493&_nc_vs=HBksFQAYJEdBVmZoUlhOX1JCY1Qxc0NBTlgyaURESkRyMTZia1lMQUFBRhUAAsgBABUAGCRHTm1ZaFJXano5Rk1UM2dBQU0wN3I2d1lrVng2YmtZTEFBQUYVAgLIAQAoABgAGwGIB3VzZV9vaWwBMBUAACa6u7r%2BmOjoPxUCKAJDMywXQDKzMzMzMzMYEmRhc2hfYmFzZWxpbmVfMl92MREAdeoHAA%3D%3D&_nc_rid=e91b3f00fc&ccb=7-5&oh=00_AfD4G2wv4eJTNJjnGXOF04FdWuUUtelx_tvm5iq5dYx1pw&oe=64B49F3C&_nc_sid=2bf346",
                                  "id": "1023383492024442v"
                              }
                          ],
                          "like_count": 9188,
                          "has_liked": false,
                          "can_viewer_reshare": true,
                          "top_likers": [],
                          "user": {
                              "pk": 9155181033,
                              "pk_id": "9155181033",
                              "username": "espnmma",
                              "full_name": "ESPN MMA",
                              "is_private": false,
                              "is_verified": true,
                              "profile_pic_id": "3141004987567665802_9155181033",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358383703_848206763406082_8252942189546556167_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=110&_nc_ohc=WOV8wy4ScRoAX_C1CaW&edm=AClQE0sBAAAA&ccb=7-5&oh=00_AfDDl35PMIh3bUJ2WuzOkIIAgIEZqwFqdSh8tuPDTyfgTg&oe=64B7C4EE&_nc_sid=2bf346",
                              "friendship_status": {
                                  "following": false,
                                  "followed_by": false,
                                  "blocking": false,
                                  "muting": false,
                                  "is_private": false,
                                  "incoming_request": false,
                                  "outgoing_request": false,
                                  "text_post_app_pre_following": false,
                                  "is_bestie": false,
                                  "is_restricted": false,
                                  "is_feed_favorite": false,
                                  "is_eligible_to_subscribe": false
                              },
                              "has_anonymous_profile_picture": false,
                              "has_onboarded_to_text_post_app": true,
                              "account_badges": []
                          }
                      },
                      "line_type": "squiggle",
                      "view_replies_cta_string": "348 replies",
                      "should_show_replies_cta": true,
                      "reply_facepile_users": [
                          {
                              "pk": 60781132585,
                              "pk_id": "60781132585",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/359641162_959966898669598_6990253892519958381_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=110&_nc_ohc=uLBckw2TPWQAX_yvvWz&edm=AClQE0sBAAAA&ccb=7-5&oh=00_AfDZeeb69FVIALEYjbqFBzIbIXMlhKhyCPMge1K-ni0hZQ&oe=64B7FC4E&_nc_sid=2bf346"
                          },
                          {
                              "pk": 57175390182,
                              "pk_id": "57175390182",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358544206_674380928039672_4729519352683894077_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=108&_nc_ohc=0qWbftDXewwAX8KUtSO&edm=AClQE0sBAAAA&ccb=7-5&oh=00_AfDj4Z6wYUVslOtd4Mp7oRhbxKJZDIsZLxjX861MFn4CfQ&oe=64B74C23&_nc_sid=2bf346"
                          },
                          {
                              "pk": 46363005289,
                              "pk_id": "46363005289",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358356136_267621489287701_6184904086615879877_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=101&_nc_ohc=_kbmWIEqHuYAX9in-BH&edm=AClQE0sBAAAA&ccb=7-5&oh=00_AfBMSnbvraqk2tIrN5MwKAT49ey9QoeppiTEiMzeggaEYw&oe=64B86906&_nc_sid=2bf346"
                          }
                      ],
                      "can_inline_expand_below": false
                  },
                  ...
              ],
              "thread_type": "thread",
              "show_create_reply_cta": false,
              "id": 3146509421211016700
          },
      ],
      "next_max_id": "QVFBZU1fRWxkXzh4UUhGOV84VUZGLS1BdHlVWVVzQmpNbmUzZ0hkRVJURXgyVDBSb2pSdnhacnRGX1JrNUxMRDgxbFZ4ZW5tSUc5TTRraEgwRV91eWR3Nw==",
      "status": "ok"
  }
  ```
</details>

##### Get Followers

`threads.private_api.get_user_followers` — getting a user's followers by the user's identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_followers = threads.private_api.get_user_followers(id=314216)
  >>> user_followers
  {
      "users": [
          {
              "has_anonymous_profile_picture": false,
              "fbid_v2": "17841407032091362",
              "has_onboarded_to_text_post_app": true,
              "text_post_app_joiner_number": 100165200,
              "pk": 6970898403,
              "pk_id": "6970898403",
              "username": "ali_moslemi_",
              "full_name": "",
              "is_private": true,
              "is_verified": false,
              "profile_pic_id": "3143675402396969798_6970898403",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358504236_795240265474093_1873793041193487951_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=101&_nc_ohc=NDKSnch-bEoAX9bRFSg&edm=APQMUHMBAAAA&ccb=7-5&oh=00_AfBAOL--evGSmg6slpcKO-Bmo1I8rXokvlMygal0yEep5A&oe=64AFB4B0&_nc_sid=6ff7c8",
              "account_badges": [],
              "is_possible_scammer": false,
              "third_party_downloads_enabled": 0,
              "is_possible_bad_actor": {
                  "is_possible_scammer": false,
                  "is_possible_impersonator": {
                      "is_unconnected_impersonator": false
                  }
              },
              "latest_reel_media": 0
          },
          ...
      ],
      "big_list": false,
      "page_size": 200,
      "has_more": false,
      "should_limit_list_of_followers": false,
      "status": "ok"
  }
  ```
</details>

##### Get Following

`threads.private_api.get_user_following` — getting a user's following by the user's identifier.

| Parameters |  Type   | Required | Restrictions | Description          |
|:----------:|:-------:|:--------:|:------------:|----------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A user's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> user_following = threads.private_api.get_user_following(id=314216)
  >>> user_following
  {
      "users": [
          {
              "has_anonymous_profile_picture": false,
              "fbid_v2": "17841400595560228",
              "has_onboarded_to_text_post_app": true,
              "text_post_app_joiner_number": 37063252,
              "pk": 14611852,
              "pk_id": "14611852",
              "username": "mikesego",
              "full_name": "Mike Sego",
              "is_private": false,
              "is_verified": false,
              "profile_pic_id": "3141060757056532902_14611852",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358004364_2046926358984696_1656910531495082901_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=100&_nc_ohc=6Hb0M2drxbIAX9eVeGw&edm=ALB854YBAAAA&ccb=7-5&oh=00_AfAeVVBHAjmjJtmatY9YTDLovtOXJwzM-netZTUN_ghOLw&oe=64B028E3&_nc_sid=ce9561",
              "account_badges": [],
              "is_possible_scammer": false,
              "third_party_downloads_enabled": 1,
              "is_possible_bad_actor": {
                  "is_possible_scammer": false,
                  "is_possible_impersonator": {
                      "is_unconnected_impersonator": false
                  }
              },
              "latest_reel_media": 0,
              "is_favorite": false
          },
          ...
      ],
      "big_list": true,
      "page_size": 200,
      "next_max_id": "100",
      "has_more": false,
      "should_limit_list_of_followers": false,
      "status": "ok"
  }
  ```
</details>

##### Get Friendship Status

`threads.private_api.get_friendship_status` — get a friendship status with a user.

| Parameters |  Type   | Required | Restrictions | Description                                            |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to get friendship status with. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> friendship_status = threads.private_api.get_friendship_status(id=314216)
  >>> friendship_status
  {
      "blocking": false,
      "followed_by": false,
      "following": false,
      "incoming_request": false,
      "is_bestie": false,
      "is_blocking_reel": false,
      "is_muting_reel": false,
      "is_private": false,
      "is_restricted": false,
      "muting": false,
      "outgoing_request": false,
      "text_post_app_pre_following": false,
      "is_feed_favorite": false,
      "is_eligible_to_subscribe": false,
      "is_supervised_by_viewer": false,
      "is_guardian_of_viewer": false,
      "is_muting_notes": false,
      "status": "ok"
  }
  ```
</details>

##### Search

`threads.private_api.search_user` — search for a user by a query.

| Parameters |  Type  | Required | Restrictions | Description     |
|:----------:|:------:|:--------:|:------------:|-----------------|
|  `query`   | String |   Yes    |      -       | A search query. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> found_users = threads.private_api.search_user(query='zuck')
  >>> found_users
  {
      "num_results": 55,
      "users": [
          {
              "has_anonymous_profile_picture": false,
              "follower_count": 2779681,
              "media_count": 283,
              "following_count": 313,
              "following_tag_count": 3,
              "fbid_v2": "17841401746480004",
              "has_onboarded_to_text_post_app": true,
              "show_text_post_app_badge": true,
              "text_post_app_joiner_number": 1,
              "show_ig_app_switcher_badge": true,
              "pk": 314216,
              "pk_id": "314216",
              "username": "zuck",
              "full_name": "Mark Zuckerberg",
              "is_private": false,
              "is_verified": true,
              "profile_pic_id": "3138909791791822006_314216",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=euIj8dtTGIkAX_8Gm2Q&edm=AM7KJZYBAAAA&ccb=7-5&oh=00_AfBozKxHv4GtcQ5ZwnPSS_eL9ONAYNekI1hhOWPGgNPaxA&oe=64B0D240&_nc_sid=8ec269",
              "has_opt_eligible_shop": false,
              "account_badges": [],
              "third_party_downloads_enabled": 1,
              "unseen_count": 0,
              "friendship_status": {
                  "following": false,
                  "is_private": false,
                  "incoming_request": false,
                  "outgoing_request": false,
                  "text_post_app_pre_following": false,
                  "is_bestie": false,
                  "is_restricted": false,
                  "is_feed_favorite": false
              },
              "latest_reel_media": 0,
              "should_show_category": false
          },
          ...
      ],
      "has_more": false,
      "rank_token": "21af3266-ddec-4166-b00c-091a580a54a8",
      "status": "ok"
  }
  ```
</details>

##### Follow

`threads.private_api.follow_user` — follow a user.

| Parameters |  Type   | Required | Restrictions | Description                        |
|:----------:|:-------:|:--------:|:------------:|------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to follow. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> following = threads.private_api.follow_user(id=314216)
  >>> following
  {
      "friendship_status": {
          "following": true,
          "followed_by": false,
          "blocking": false,
          "muting": false,
          "is_private": false,
          "incoming_request": false,
          "outgoing_request": false,
          "text_post_app_pre_following": false,
          "is_bestie": false,
          "is_restricted": false,
          "is_feed_favorite": false,
          "is_eligible_to_subscribe": false
      },
      "previous_following": false,
      "status": "ok"
  }
  ```
</details>

##### Unfollow

`threads.private_api.unfollow_user` — unfollow a user.

| Parameters |  Type   | Required | Restrictions | Description                          |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to unfollow. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> following = threads.private_api.unfollow_user(id=314216)
  >>> following
  {
      "friendship_status": {
          "following": false,
          "followed_by": false,
          "blocking": false,
          "muting": false,
          "is_private": false,
          "incoming_request": false,
          "outgoing_request": false,
          "text_post_app_pre_following": false,
          "is_bestie": false,
          "is_restricted": false,
          "is_feed_favorite": false,
          "is_eligible_to_subscribe": false
      },
      "status": "ok"
  }
  ```
</details>

##### Mute

`threads.private_api.mute_user` — mute a user.

| Parameters |  Type   | Required | Restrictions | Description                      |
|:----------:|:-------:|:--------:|:------------:|----------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to mute. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> muting = threads.private_api.mute_user(id=314216)
  >>> muting
  {
      "friendship_status": {
          "muting": true,
          ...
      },
      "status": "ok"
  }
  ```
</details>

##### Unmute

`threads.private_api.unmute_user` — unmute a user.

| Parameters |  Type   | Required | Restrictions | Description                        |
|:----------:|:-------:|:--------:|:------------:|------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to unmute. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> unmuting = threads.private_api.unmute_user(id=314216)
  >>> unmuting
  {
      "friendship_status": {
          "muting": false,
          ...
      },
      "status": "ok"
  }
  ```
</details>

##### Restrict

`threads.private_api.restrict_user` — restrict a user.

| Parameters |  Type   | Required | Restrictions | Description                          |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to restrict. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> restricting = threads.private_api.restrict_user(id=314216)
  >>> restricting
  {
      "users": [
          {
              "pk": 314216,
              "pk_id": "314216",
              "username": "zuck",
              "full_name": "Mark Zuckerberg",
              "is_private": false,
              "is_verified": true,
              "friendship_status": {
                  "following": false,
                  "followed_by": false,
                  "blocking": false,
                  "muting": false,
                  "is_private": false,
                  "incoming_request": false,
                  "outgoing_request": false,
                  "text_post_app_pre_following": false,
                  "is_bestie": false,
                  "is_restricted": true,
                  "is_feed_favorite": false,
                  "is_eligible_to_subscribe": false
              },
              "profile_pic_id": "3138909791791822006_314216",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=XKEMOZTrwUAAX-1hCyT&edm=AIbK1RIBAAAA&ccb=7-5&oh=00_AfDM-zL-HYX9qBBi9zM3AaHFAFzGu5ASanTF60ahGOYh7Q&oe=64B4C6C0&_nc_sid=0e3124",
              "has_onboarded_to_text_post_app": true
          }
      ],
      "status": "ok"
  }
  ```
</details>

##### Unrestrict

`threads.private_api.unrestrict_user` — unrestrict a user.

| Parameters |  Type   | Required | Restrictions | Description                            |
|:----------:|:-------:|:--------:|:------------:|----------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to unrestrict. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> unrestricting = threads.private_api.unrestrict_user(id=314216)
  >>> unrestricting
  {
      "users": [
          {
              "pk": 314216,
              "pk_id": "314216",
              "username": "zuck",
              "full_name": "Mark Zuckerberg",
              "is_private": false,
              "is_verified": true,
              "friendship_status": {
                  "following": false,
                  "followed_by": false,
                  "blocking": false,
                  "muting": false,
                  "is_private": false,
                  "incoming_request": false,
                  "outgoing_request": false,
                  "text_post_app_pre_following": false,
                  "is_bestie": false,
                  "is_restricted": false,
                  "is_feed_favorite": false,
                  "is_eligible_to_subscribe": false
              },
              "profile_pic_id": "3138909791791822006_314216",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/357376107_1330597350674698_8884059223384672080_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=XKEMOZTrwUAAX-1hCyT&edm=AMkHgcoBAAAA&ccb=7-5&oh=00_AfAMSrQmoJc3wyGdpGegznMp-f00XjYVyHNgr26As8rRHQ&oe=64B4C6C0&_nc_sid=88bf82",
              "has_onboarded_to_text_post_app": true
          }
      ],
      "status": "ok"
  }
  ```
</details>

##### Block

`threads.private_api.block_user` — block a user.

| Parameters |  Type   | Required | Restrictions | Description                       |
|:----------:|:-------:|:--------:|:------------:|-----------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to block. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> blocking = threads.private_api.block_user(id=314216)
  >>> blocking
  {
      "friendship_status": {
          "blocking": true,
          ...
      },
      "status": "ok"
  }
  ```
</details>

##### Unblock

`threads.private_api.unblock_user` — unblock a user.

| Parameters |  Type   | Required | Restrictions | Description                         |
|:----------:|:-------:|:--------:|:------------:|-------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a user to unblock. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> unblocking = threads.private_api.unblock_user(id=314216)
  >>> unblocking
  {
    "friendship_status": {
        "blocking": false,
        ...
    },
    "status": "ok"
}
  ```
</details>

#### Thread

##### Get Identifier

`threads.private_api.get_thread_id` — getting a thread's identifier by its `URL` identifier. `URL` identifier is a last 
part of a thread's website `URL`. If the thread's `URL` is `https://threads.net/t/CuXFPIeLLod`, then it would be `CuXFPIeLLod`.

| Parameters |  Type  | Required | Restrictions | Description                  |
|:----------:|:------:|:--------:|:------------:|------------------------------|
|  `url_id`  | String |   Yes    |      -       | A thread's `URL` identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread_id = threads.private_api.get_thread_id(url_id='CuXFPIeLLod')
  >>> thread_id
  3141002295235099165
  ```
</details>

##### Get By Identifier

`threads.private_api.get_thread` — getting a thread by its identifier.

| Parameters |  Type   | Required | Restrictions | Description            |
|:----------:|:-------:|:--------:|:------------:|------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A thread's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread = threads.private_api.get_thread(id=3141002295235099165)
  >>> thread
  {
      "data": {
          "data": {
              "containing_thread": {
                  "thread_items": [
                      {
                          "post": {
                              "user": {
                                  "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/343392897_618515990300243_8088199406170073086_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=1&_nc_ohc=1xtR491RY6cAX8Okf3Z&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfDqsZTCRQlz7EaDpgEzBkmXqtqKtTH80Q0r1rIDMJCcpg&oe=64AC2D8D&_nc_sid=10d13b",
                                  "username": "mosseri",
                                  "id": null,
                                  "is_verified": true,
                                  "pk": "95561"
                              },
                              "image_versions2": {
                                  "candidates": []
                              },
                              "original_width": 612,
                              "original_height": 612,
                              "video_versions": [],
                              "carousel_media": null,
                              "carousel_media_count": null,
                              "pk": "3141055616164096839",
                              "has_audio": null,
                              "text_post_app_info": {
                                  "link_preview_attachment": null,
                                  "share_info": {
                                      "quoted_post": null,
                                      "reposted_post": null
                                  },
                                  "reply_to_author": null,
                                  "is_post_unavailable": false,
                                  "direct_reply_count": 2839
                              },
                              "caption": {
                                  "text": "I've been getting some questions about deleting your account. To clarify, you can deactivate your Threads account, which hides your Threads profile and content, you can set your profile to private, and you can delete individual threads posts \u2013 all without deleting your Instagram account. Threads is powered by Instagram, so right now it's just one account, but we're looking into a way to delete your Threads account separately."
                              },
                              "taken_at": 1688663030,
                              "like_count": 29984,
                              "code": "CuXRXDdNOtH",
                              "media_overlay_info": null,
                              "id": "3141055616164096839_95561"
                          },
                          "line_type": "none",
                          "view_replies_cta_string": "2,839 replies",
                          "reply_facepile_users": [],
                          "should_show_replies_cta": true
                      }
                  ],
                  "thread_type": "thread",
                  "header": null,
                  "id": "3141055616164096839"
              },
              "reply_threads": [
                  {
                      "thread_items": [
                          {
                              "post": {
                                  "user": {
                                      "profile_pic_url": "https://scontent.cdninstagram.com/v/t51.2885-19/355424589_948742193018772_4617955233166333765_n.jpg?stp=dst-jpg_s150x150&_nc_ht=scontent.cdninstagram.com&_nc_cat=100&_nc_ohc=heQWdfsKG5MAX9mLim0&edm=APs17CUBAAAA&ccb=7-5&oh=00_AfA6TUpLjjegnP6KuducRm8n6uU9iBCXg4O1P33WYSh3mg&oe=64ACDDBA&_nc_sid=10d13b",
                                      "username": "jimmywhotalks",
                                      "id": null,
                                      "is_verified": true,
                                      "pk": "51094265817"
                                  },
                                  "image_versions2": {
                                      "candidates": []
                                  },
                                  "original_width": 612,
                                  "original_height": 612,
                                  "video_versions": [],
                                  "carousel_media": null,
                                  "carousel_media_count": null,
                                  "pk": "3141082664316809193",
                                  "has_audio": null,
                                  "text_post_app_info": {
                                      "link_preview_attachment": null,
                                      "share_info": {
                                          "quoted_post": null,
                                          "reposted_post": null
                                      },
                                      "reply_to_author": {
                                          "username": "mosseri",
                                          "id": null
                                      },
                                      "is_post_unavailable": false
                                  },
                                  "caption": {
                                      "text": "Glad to know, Everyone is worrying for that."
                                  },
                                  "taken_at": 1688666254,
                                  "like_count": 59,
                                  "code": "CuXXgqAva_p",
                                  "media_overlay_info": null,
                                  "id": "3141082664316809193_51094265817"
                              },
                              "line_type": "none",
                              "view_replies_cta_string": null,
                              "reply_facepile_users": [],
                              "should_show_replies_cta": false
                          }
                      ],
                      "thread_type": "thread",
                      "header": null,
                      "id": "3141082664316809193"
                  },
                  ...
              ]
          }
      },
      "extensions": {
          "is_final": true
      }
  }
  ```
</details>

##### Get Likers

`threads.private_api.get_thread_likers` — getting a thread's likers by the thread's identifier.

| Parameters |  Type   | Required | Restrictions | Description            |
|:----------:|:-------:|:--------:|:------------:|------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | A thread's identifier. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> thread_likers = threads.private_api.get_thread_likers(id=3141055616164096839)
  >>> thread_likers
  {
      "users": [
          {
              "pk": 54831820289,
              "pk_id": "54831820289",
              "username": "hazrel_syfddn26",
              "full_name": "HAZLER",
              "is_private": false,
              "account_badges": [],
              "is_verified": false,
              "profile_pic_id": "3140964514487728732_54831820289",
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358165944_6109031802541294_1514688336008099084_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=110&_nc_ohc=V1UF-ylegwMAX_HbWIV&edm=APwHDrQBAAAA&ccb=7-5&oh=00_AfC4W3z1EZ61XFQk6Im82OpgoEvQp_r-JNPd2NmN9cdhUQ&oe=64B23019&_nc_sid=8809c9",
              "has_onboarded_to_text_post_app": true,
              "latest_reel_media": 0
          },
          ...
      ],
      "user_count": 38283,
      "status": "ok"
  }
  ```
</details>

##### Create

`threads.private_api.create_thread` — create a thread. You can create a thread with an attachment link or image
(specifying either `HTTP(S)` `URL` or path to a file). Also, you are able to create a thread as a reply to another
thread. Basically, each thread is either a root or linked to another thread like a graph. You can check 
`https://www.threads.net/@threadstester1` for all the illustration of possible threads that can be created.

| Parameters  |  Type  | Required | Restrictions | Description                                   |
|:-----------:|:------:|:--------:|:------------:|-----------------------------------------------|
|  `caption`  | String |   Yes    |      -       | A thread's caption.                           |
|    `url`    | String |    No    |      -       | A thread's attachment `URL`.                  |
| `image_url` | String |    No    |      -       | An image's `HTTP(S)` `URL` or path to a file. |
| `reply_to`  | String |    No    |      -       | An identifier of a thread to reply to.        |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> created_thread = threads.private_api.create_thread(
          caption='Hello, world!',
      )
  >>> created_thread = threads.private_api.create_thread(
          caption='Hello, world!',
          url='https://www.youtube.com/watch?v=lc4qU6BakvE'
      )
  >>> created_thread = threads.private_api.create_thread(
          caption='Hello, world!',
          image_url='/Users/dmytrostriletskyi/projects/threads/assets/picture.png',
      )
  >>> created_thread = threads.private_api.create_thread(
          caption='Hello, world!',
          image_url='https://raw.githubusercontent.com/dmytrostriletskyi/threads-net/main/assets/picture.png',
          reply_to=3141055616164096839,
      )
  >>> created_thread
  {
      "media": {
          "taken_at": 1689087793,
          "pk": 3144618785809596584,
          "id": "3144618785809596584_32545771157",
          "device_timestamp": 1689087791,
          "media_type": 1,
          "code": "Cuj7h_yM4Co",
          "client_cache_key": "MzE0NDYxODc4NTgwOTU5NjU4NA==.2",
          "filter_type": 0,
          "can_viewer_reshare": true,
          "caption": {
              "pk": "17986860047032912",
              "user_id": 32545771157,
              "text": "Hello, world!",
              "type": 1,
              "created_at": 1689087793,
              "created_at_utc": 1689087793,
              "content_type": "comment",
              "status": "Active",
              "bit_flags": 0,
              "did_report_as_spam": false,
              "share_enabled": false,
              "user": {
                  "has_anonymous_profile_picture": true,
                  "liked_clips_count": 0,
                  "fan_club_info": {
                      "fan_club_id": null,
                      "fan_club_name": null,
                      "is_fan_club_referral_eligible": null,
                      "fan_consideration_page_revamp_eligiblity": null,
                      "is_fan_club_gifting_eligible": null,
                      "subscriber_count": null,
                      "connected_member_count": null,
                      "autosave_to_exclusive_highlight": null,
                      "has_enough_subscribers_for_ssc": null
                  },
                  "fbid_v2": 17841432494728221,
                  "transparency_product_enabled": false,
                  "text_post_app_take_a_break_setting": 0,
                  "interop_messaging_user_fbid": 17842937552083158,
                  "show_insights_terms": false,
                  "allowed_commenter_type": "any",
                  "is_unpublished": false,
                  "reel_auto_archive": "unset",
                  "can_boost_post": false,
                  "can_see_organic_insights": false,
                  "has_onboarded_to_text_post_app": true,
                  "text_post_app_joiner_number": 68427510,
                  "pk": 32545771157,
                  "pk_id": "32545771157",
                  "username": "dmytro.striletskyi",
                  "full_name": "Dmytro Striletskyi",
                  "is_private": false,
                  "profile_pic_url": "https://scontent-cdg4-1.cdninstagram.com/v/t51.2885-19/44884218_345707102882519_2446069589734326272_n.jpg?_nc_ht=scontent-cdg4-1.cdninstagram.com&_nc_cat=1&_nc_ohc=s7XO4bcYceYAX9jyeT8&edm=AAAAAAABAAAA&ccb=7-5&ig_cache_key=YW5vbnltb3VzX3Byb2ZpbGVfcGlj.2-ccb7-5&oh=00_AfBhetyTgz8h53kRlSorYqET2tbmZOmYt2jiucME8qI7AQ&oe=64B3528F",
                  "account_badges": [],
                  "feed_post_reshare_disabled": false,
                  "show_account_transparency_details": true,
                  "third_party_downloads_enabled": 0
              },
              "is_covered": false,
              "is_ranked_comment": false,
              "media_id": 3144618785809596584,
              "private_reply_status": 0
          },
          "clips_tab_pinned_user_ids": [],
          "comment_inform_treatment": {
              "should_have_inform_treatment": false,
              "text": "",
              "url": null,
              "action_type": null
          },
          "fundraiser_tag": {
              "has_standalone_fundraiser": false
          },
          "sharing_friction_info": {
              "should_have_sharing_friction": false,
              "bloks_app_url": null,
              "sharing_friction_payload": null
          },
          "xpost_deny_reason": "This post cannot be shared to Facebook.",
          "caption_is_edited": false,
          "original_media_has_visual_reply_media": false,
          "like_and_view_counts_disabled": false,
          "fb_user_tags": {
              "in": []
          },
          "mashup_info": {
              "mashups_allowed": true,
              "can_toggle_mashups_allowed": true,
              "has_been_mashed_up": false,
              "formatted_mashups_count": null,
              "original_media": null,
              "privacy_filtered_mashups_media_count": null,
              "non_privacy_filtered_mashups_media_count": null,
              "mashup_type": null,
              "is_creator_requesting_mashup": false,
              "has_nonmimicable_additional_audio": false,
              "is_pivot_page_available": false
          },
          "can_viewer_save": true,
          "is_in_profile_grid": false,
          "profile_grid_control_enabled": false,
          "featured_products": [],
          "is_comments_gif_composer_enabled": true,
          "product_suggestions": [],
          "user": {
              "has_anonymous_profile_picture": true,
              "liked_clips_count": 0,
              "fan_club_info": {
                  "fan_club_id": null,
                  "fan_club_name": null,
                  "is_fan_club_referral_eligible": null,
                  "fan_consideration_page_revamp_eligiblity": null,
                  "is_fan_club_gifting_eligible": null,
                  "subscriber_count": null,
                  "connected_member_count": null,
                  "autosave_to_exclusive_highlight": null,
                  "has_enough_subscribers_for_ssc": null
              },
              "fbid_v2": 17841432494728221,
              "transparency_product_enabled": false,
              "text_post_app_take_a_break_setting": 0,
              "interop_messaging_user_fbid": 17842937552083158,
              "show_insights_terms": false,
              "allowed_commenter_type": "any",
              "is_unpublished": false,
              "reel_auto_archive": "unset",
              "can_boost_post": false,
              "can_see_organic_insights": false,
              "has_onboarded_to_text_post_app": true,
              "text_post_app_joiner_number": 68427510,
              "pk": 32545771157,
              "pk_id": "32545771157",
              "username": "dmytro.striletskyi",
              "full_name": "Dmytro Striletskyi",
              "is_private": false,
              "profile_pic_url": "https://scontent-cdg4-1.cdninstagram.com/v/t51.2885-19/44884218_345707102882519_2446069589734326272_n.jpg?_nc_ht=scontent-cdg4-1.cdninstagram.com&_nc_cat=1&_nc_ohc=s7XO4bcYceYAX9jyeT8&edm=AAAAAAABAAAA&ccb=7-5&ig_cache_key=YW5vbnltb3VzX3Byb2ZpbGVfcGlj.2-ccb7-5&oh=00_AfBhetyTgz8h53kRlSorYqET2tbmZOmYt2jiucME8qI7AQ&oe=64B3528F",
              "account_badges": [],
              "feed_post_reshare_disabled": false,
              "show_account_transparency_details": true,
              "third_party_downloads_enabled": 0
          },
          "image_versions2": {
              "candidates": [
                  {
                      "width": 980,
                      "height": 652,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfB5avonpo72-x_5Te_cXCKf2aM6jJO9dGmmejvdV821zg&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 720,
                      "height": 479,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35_s720x720&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfBJhcGMT6yd6pECPVoeFAIoTIcgOzTw6zDKxs8ZdZK1CA&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 640,
                      "height": 426,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfBPn-IvLSL4PYxQQoFyFyHFuimEtfKoDq7EoCys3xaMWg&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 480,
                      "height": 319,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35_s480x480&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfDiPNhb5u_FX3VKC8KZ1rmN7yvmmTMiunWJBK25yuly4A&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 320,
                      "height": 213,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35_s320x320&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfAy8wZuJneP3N_g6PyDKk-2kiAGLvPO4XPfK1MBwVSSPw&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 240,
                      "height": 160,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=dst-jpg_e35_s240x240&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfDnUnB202rqbhfPupeNKKd-NRM9e4-FvsnnGD2FsEY_bQ&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 1080,
                      "height": 1080,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfDGwvzJe8hIAfBBrRerzo8PCdQAey0_Q9uJSF5eGz1f-w&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 750,
                      "height": 750,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s750x750_sh0.08&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfD2nnAd_T-Zj3cX0-TUEjajO9jn3EMfKwaUHwLmagBhiA&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 640,
                      "height": 640,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfB0kVjlsVkk26Rqad6KRlMWTOxs88NzLS1iuHpHYLU1tA&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 480,
                      "height": 480,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s480x480&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfCo8QoGPw8tlIavEsSbZkMoZOFdey8XM0--RAIABYqxZQ&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 320,
                      "height": 320,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s320x320&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfAfleZMSb3Ae50Fj1dhDUYc6slUO5zxUKwcKYL0PmHNkA&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 240,
                      "height": 240,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s240x240&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfAR1f9S-NF_DwhyaSq0qzme6k4rDsGb6RfWkgnw07Tt1A&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  },
                  {
                      "width": 150,
                      "height": 150,
                      "url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-15/359145974_1454822158673126_2191780209015799278_n.jpg?stp=c164.0.652.652a_dst-jpg_e35_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=8vV5VulJfUwAX-jnGZU&edm=AL_hgCEBAAAA&ccb=7-5&ig_cache_key=MzE0NDYxODc4NTgwOTU5NjU4NA%3D%3D.2-ccb7-5&oh=00_AfAYtetjgt4tQah5PnX8LXRlrUGS0A1uH4kkUrUPNwcr_g&oe=64B3067E&_nc_sid=e50d24",
                      "scans_profile": "e35"
                  }
              ]
          },
          "original_width": 980,
          "original_height": 652,
          "is_reshare_of_text_post_app_media_in_ig": false,
          "comment_threading_enabled": false,
          "max_num_visible_preview_comments": 2,
          "has_more_comments": false,
          "preview_comments": [],
          "comment_count": 0,
          "can_view_more_preview_comments": false,
          "hide_view_all_comment_entrypoint": false,
          "likers": [],
          "shop_routing_user_id": null,
          "can_see_insights_as_brand": false,
          "is_organic_product_tagging_eligible": false,
          "product_type": "feed",
          "is_paid_partnership": false,
          "music_metadata": {
              "music_canonical_id": "0",
              "audio_type": null,
              "music_info": null,
              "original_sound_info": null,
              "pinned_media_ids": null
          },
          "deleted_reason": 0,
          "organic_tracking_token": "eyJ2ZXJzaW9uIjo1LCJwYXlsb2FkIjp7ImlzX2FuYWx5dGljc190cmFja2VkIjpmYWxzZSwidXVpZCI6IjQxNjBkY2E0ODY1YzQyODY5NThhOWE3M2I2N2UyZWI0MzE0NDYxODc4NTgwOTU5NjU4NCIsInNlcnZlcl90b2tlbiI6IjE2ODkwODc3OTYwOTZ8MzE0NDYxODc4NTgwOTU5NjU4NHwzMjU0NTc3MTE1N3xiY2RkZjliZGNlZjFiMzNlZDIzZTJiZDg4M2E0MjAzZmNlMjQ1ZjI2MjdmYWZiNDgwNTlhNjBiMzgwYjM1MjA3In0sInNpZ25hdHVyZSI6IiJ9",
          "text_post_app_info": {
              "is_post_unavailable": false,
              "is_reply": true,
              "reply_to_author": {
                  "pk": 95561,
                  "pk_id": "95561",
                  "username": "mosseri",
                  "full_name": "Adam Mosseri",
                  "is_private": false,
                  "is_verified": true,
                  "profile_pic_id": "3090458139926225297_95561",
                  "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/343392897_618515990300243_8088199406170073086_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=H93HRKonYSQAX9OmlMQ&edm=AL_hgCEBAAAA&ccb=7-5&oh=00_AfC8sdQl6N1YwEs5Xva8HSfc98_zv7l4beWBQVuVBkHxJA&oe=64B21C4D&_nc_sid=e50d24",
                  "has_onboarded_to_text_post_app": true
              },
              "direct_reply_count": 0,
              "self_thread_count": 0,
              "reply_facepile_users": [],
              "link_preview_attachment": null,
              "can_reply": true,
              "reply_control": "everyone",
              "hush_info": null,
              "share_info": {
                  "can_repost": true,
                  "is_reposted_by_viewer": false,
                  "can_quote_post": true
              }
          },
          "integrity_review_decision": "pending",
          "ig_media_sharing_disabled": false,
          "has_shared_to_fb": 0,
          "is_unified_video": false,
          "should_request_ads": false,
          "is_visual_reply_commenter_notice_enabled": true,
          "commerciality_status": "not_commercial",
          "explore_hide_comments": false,
          "has_delayed_metadata": false
      },
      "upload_id": "1689087791",
      "status": "ok"
  }
  ```
</details>

<details>
  <summary>Open UI example</summary>

  ![](./assets/examples/create-thread-caption.png)
  ![](./assets/examples/create-thread-url.png)
  ![](./assets/examples/create-thread-image.png)
</details>

##### Delete

`threads.private_api.delete_thread` — delete a thread.

| Parameters |  Type   | Required | Restrictions | Description                          |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to delete. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> deletion = threads.private_api.delete_thread(id=3141055616164096839)
  >>> deletion
  {
      "did_delete": true,
      "cxp_deep_deletion_global_response": {},
      "status": "ok"
  }
  ```
</details>

##### Like

`threads.private_api.like_thread` — like a thread.

| Parameters |  Type   | Required | Restrictions | Description                        |
|:----------:|:-------:|:--------:|:------------:|------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to like. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> liking = threads.private_api.like_thread(id=3141055616164096839)
  >>> liking
  {
      "status": "ok"
  }
  ```
</details>

##### Unlike

`threads.private_api.unlike_thread` — unlike a thread.

| Parameters |  Type   | Required | Restrictions | Description                          |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to unlike. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> unliking = threads.private_api.unlike_thread(id=3141055616164096839)
  >>> unliking
  {
      "status": "ok"
  }
  ```
</details>

##### Repost

`threads.private_api.repost_thread` — repost a thread.

| Parameters |  Type   | Required | Restrictions | Description                          |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to repost. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> reposting = threads.private_api.repost_thread(id=3141055616164096839)
  >>> reposting
  {
      "repost_id": 3145900181542784653,
      "repost_fbid": 18008518438811136,
      "reposted_at": 1689240547,
      "status": "ok"
  }
  ```
</details>

<details>
  <summary>Open UI example</summary>

  ![](./assets/examples/repost-thread.png)
</details>

##### Unrepost

`threads.private_api.unrepost_thread` — undo a thread's repost. An identifier of a thread to unrepost is basically the
identifier of the thread from `repost` method.

| Parameters |  Type   | Required | Restrictions | Description                            |
|:----------:|:-------:|:--------:|:------------:|----------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to unrepost. |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> unreposting = threads.private_api.unrepost_thread(id=3141055616164096839)
  >>> unreposting
  {
      "status": "ok"
  }
  ```
</details>

##### Quote

`threads.private_api.quote_thread` — quote a thread.

| Parameters |  Type   | Required | Restrictions | Description                         |
|:----------:|:-------:|:--------:|:------------:|-------------------------------------|
|    `id`    | Integer |   Yes    |     `>0`     | An identifier of a thread to quote. |
| `caption`  | String  |   Yes    |      -       | A quote's caption.                  |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> quoting = threads.private_api.quote_thread(id=3141055616164096839, caption='Hello, world!')
  >>> quoting
  {
      "media": {
          "taken_at": 1689241970,
          "pk": 3145912119832282782,
          "id": "3145912119832282782_60438154566",
          "device_timestamp": 1689241970,
          "media_type": 19,
          "code": "Cuohme9Mwae",
          "client_cache_key": "MzE0NTkxMjExOTgzMjI4Mjc4Mg==.2",
          "filter_type": 0,
          "can_viewer_reshare": true,
          "caption": {
              "pk": "18086087680362532",
              "user_id": 60438154566,
              "text": "Hello, world!",
              "type": 1,
              "created_at": 1689241971,
              "created_at_utc": 1689241971,
              "content_type": "comment",
              "status": "Active",
              "bit_flags": 0,
              "did_report_as_spam": false,
              "share_enabled": false,
              "user": {
                  "has_anonymous_profile_picture": true,
                  "all_media_count": 33,
                  "liked_clips_count": 0,
                  "fan_club_info": {
                      "fan_club_id": null,
                      "fan_club_name": null,
                      "is_fan_club_referral_eligible": null,
                      "fan_consideration_page_revamp_eligiblity": null,
                      "is_fan_club_gifting_eligible": null,
                      "subscriber_count": null,
                      "connected_member_count": null,
                      "autosave_to_exclusive_highlight": null,
                      "has_enough_subscribers_for_ssc": null
                  },
                  "fbid_v2": 17841460317004769,
                  "transparency_product_enabled": false,
                  "text_post_app_take_a_break_setting": 0,
                  "interop_messaging_user_fbid": 17848558452010567,
                  "show_insights_terms": false,
                  "allowed_commenter_type": "any",
                  "is_unpublished": false,
                  "reel_auto_archive": "unset",
                  "can_boost_post": false,
                  "can_see_organic_insights": false,
                  "has_onboarded_to_text_post_app": true,
                  "text_post_app_joiner_number": 95787975,
                  "pk": 60438154566,
                  "pk_id": "60438154566",
                  "username": "threadstester1",
                  "full_name": "threads tester",
                  "is_private": false,
                  "profile_pic_url": "https://scontent-lhr8-1.cdninstagram.com/v/t51.2885-19/44884218_345707102882519_2446069589734326272_n.jpg?_nc_ht=scontent-lhr8-1.cdninstagram.com&_nc_cat=1&_nc_ohc=d9GMadcVaNMAX8OxeS1&edm=AAAAAAABAAAA&ccb=7-5&ig_cache_key=YW5vbnltb3VzX3Byb2ZpbGVfcGlj.2-ccb7-5&oh=00_AfCgI100IU2KFElc0Xo-ngiCYBS2chBfBxpchDkHna42Lw&oe=64B54CCF",
                  "account_badges": [],
                  "feed_post_reshare_disabled": false,
                  "show_account_transparency_details": true,
                  "third_party_downloads_enabled": 0
              },
              "is_covered": false,
              "is_ranked_comment": false,
              "media_id": 3145912119832282782,
              "private_reply_status": 0
          },
          "clips_tab_pinned_user_ids": [],
          "comment_inform_treatment": {
              "should_have_inform_treatment": false,
              "text": "",
              "url": null,
              "action_type": null
          },
          "fundraiser_tag": {
              "has_standalone_fundraiser": false
          },
          "sharing_friction_info": {
              "should_have_sharing_friction": false,
              "bloks_app_url": null,
              "sharing_friction_payload": null
          },
          "xpost_deny_reason": "This post cannot be shared to Facebook.",
          "caption_is_edited": false,
          "original_media_has_visual_reply_media": false,
          "like_and_view_counts_disabled": false,
          "fb_user_tags": {
              "in": []
          },
          "can_viewer_save": true,
          "is_in_profile_grid": false,
          "profile_grid_control_enabled": false,
          "featured_products": [],
          "is_comments_gif_composer_enabled": true,
          "product_suggestions": [],
          "user": {
              "has_anonymous_profile_picture": true,
              "all_media_count": 33,
              "liked_clips_count": 0,
              "fan_club_info": {
                  "fan_club_id": null,
                  "fan_club_name": null,
                  "is_fan_club_referral_eligible": null,
                  "fan_consideration_page_revamp_eligiblity": null,
                  "is_fan_club_gifting_eligible": null,
                  "subscriber_count": null,
                  "connected_member_count": null,
                  "autosave_to_exclusive_highlight": null,
                  "has_enough_subscribers_for_ssc": null
              },
              "fbid_v2": 17841460317004769,
              "transparency_product_enabled": false,
              "text_post_app_take_a_break_setting": 0,
              "interop_messaging_user_fbid": 17848558452010567,
              "show_insights_terms": false,
              "allowed_commenter_type": "any",
              "is_unpublished": false,
              "reel_auto_archive": "unset",
              "can_boost_post": false,
              "can_see_organic_insights": false,
              "has_onboarded_to_text_post_app": true,
              "text_post_app_joiner_number": 95787975,
              "pk": 60438154566,
              "pk_id": "60438154566",
              "username": "threadstester1",
              "full_name": "threads tester",
              "is_private": false,
              "profile_pic_url": "https://scontent-lhr8-1.cdninstagram.com/v/t51.2885-19/44884218_345707102882519_2446069589734326272_n.jpg?_nc_ht=scontent-lhr8-1.cdninstagram.com&_nc_cat=1&_nc_ohc=d9GMadcVaNMAX8OxeS1&edm=AAAAAAABAAAA&ccb=7-5&ig_cache_key=YW5vbnltb3VzX3Byb2ZpbGVfcGlj.2-ccb7-5&oh=00_AfCgI100IU2KFElc0Xo-ngiCYBS2chBfBxpchDkHna42Lw&oe=64B54CCF",
              "account_badges": [],
              "feed_post_reshare_disabled": false,
              "show_account_transparency_details": true,
              "third_party_downloads_enabled": 0
          },
          "image_versions2": {
              "candidates": []
          },
          "original_width": 612,
          "original_height": 612,
          "is_reshare_of_text_post_app_media_in_ig": false,
          "comment_threading_enabled": false,
          "max_num_visible_preview_comments": 2,
          "has_more_comments": false,
          "preview_comments": [],
          "comment_count": 0,
          "can_view_more_preview_comments": false,
          "hide_view_all_comment_entrypoint": false,
          "likers": [],
          "shop_routing_user_id": null,
          "can_see_insights_as_brand": false,
          "is_organic_product_tagging_eligible": false,
          "product_type": "text_post",
          "is_paid_partnership": false,
          "music_metadata": null,
          "deleted_reason": 0,
          "organic_tracking_token": "eyJ2ZXJzaW9uIjo1LCJwYXlsb2FkIjp7ImlzX2FuYWx5dGljc190cmFja2VkIjpmYWxzZSwidXVpZCI6ImY4MWVhNDFlYmY2NjRmNWNiOGUxMDM1ZWVlOTBmMjY3MzE0NTkxMjExOTgzMjI4Mjc4MiIsInNlcnZlcl90b2tlbiI6IjE2ODkyNDE5NzI5Nzd8MzE0NTkxMjExOTgzMjI4Mjc4Mnw2MDQzODE1NDU2Nnw2NzczNzc1YjJmODljNDhiNGI4ZTBlYTlmODYxYzA5NWRiMTc2ODYwOWRmYWIzMmI1OGUyYmY4OWM5MmFkZTlmIn0sInNpZ25hdHVyZSI6IiJ9",
          "text_post_app_info": {
              "is_post_unavailable": false,
              "is_reply": false,
              "reply_to_author": null,
              "direct_reply_count": 0,
              "self_thread_count": 0,
              "reply_facepile_users": [],
              "link_preview_attachment": null,
              "can_reply": true,
              "reply_control": "everyone",
              "hush_info": null,
              "share_info": {
                  "can_repost": true,
                  "is_reposted_by_viewer": false,
                  "can_quote_post": true,
                  "quoted_post": {
                      "pk": 3141055616164096839,
                      "id": "3141055616164096839_95561",
                      "text_post_app_info": {
                          "is_post_unavailable": false,
                          "is_reply": false,
                          "reply_to_author": null,
                          "direct_reply_count": 3513,
                          "self_thread_count": 0,
                          "reply_facepile_users": [],
                          "link_preview_attachment": null,
                          "can_reply": true,
                          "reply_control": "everyone",
                          "hush_info": null,
                          "share_info": {
                              "can_repost": true,
                              "is_reposted_by_viewer": false,
                              "can_quote_post": true
                          }
                      },
                      "caption": {
                          "pk": "17990855999107063",
                          "user_id": 95561,
                          "text": "I've been getting some questions about deleting your account. To clarify, you can deactivate your Threads account, which hides your Threads profile and content, you can set your profile to private, and you can delete individual threads posts \u2013 all without deleting your Instagram account. Threads is powered by Instagram, so right now it's just one account, but we're looking into a way to delete your Threads account separately.",
                          "type": 1,
                          "created_at": 1688663030,
                          "created_at_utc": 1688663030,
                          "content_type": "comment",
                          "status": "Active",
                          "bit_flags": 0,
                          "did_report_as_spam": false,
                          "share_enabled": false,
                          "user": {
                              "pk": 95561,
                              "pk_id": "95561",
                              "username": "mosseri",
                              "full_name": "Adam Mosseri",
                              "is_private": false,
                              "is_verified": true,
                              "profile_pic_id": "3090458139926225297_95561",
                              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/343392897_618515990300243_8088199406170073086_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=HX3vm4L7D44AX_09srG&edm=AMl8KrABAAAA&ccb=7-5&oh=00_AfBQ5qRqYCwxeopGHTpXx0LsYGnxpWEapsnHJpdkFruvEA&oe=64B4168D&_nc_sid=cd9b4f",
                              "fbid_v2": "17841400946830001",
                              "has_onboarded_to_text_post_app": true
                          },
                          "is_covered": false,
                          "is_ranked_comment": false,
                          "media_id": 3141055616164096839,
                          "private_reply_status": 0
                      },
                      "taken_at": 1688663030,
                      "device_timestamp": 28775508833101,
                      "media_type": 19,
                      "code": "CuXRXDdNOtH",
                      "client_cache_key": "MzE0MTA1NTYxNjE2NDA5NjgzOQ==.2",
                      "filter_type": 0,
                      "product_type": "text_post",
                      "organic_tracking_token": "eyJ2ZXJzaW9uIjo1LCJwYXlsb2FkIjp7ImlzX2FuYWx5dGljc190cmFja2VkIjp0cnVlLCJ1dWlkIjoiZjgxZWE0MWViZjY2NGY1Y2I4ZTEwMzVlZWU5MGYyNjczMTQxMDU1NjE2MTY0MDk2ODM5Iiwic2VydmVyX3Rva2VuIjoiMTY4OTI0MTk3MjYzNHwzMTQxMDU1NjE2MTY0MDk2ODM5fDYwNDM4MTU0NTY2fDUxZTliM2JkZjQ3ZDdkMDAyZTZlOGJmZjA0NDY2YjM3NWM4ZjQ1Yzc4YzYzMmNlYThlYmYwNDcyNDlkMDI1NzUifSwic2lnbmF0dXJlIjoiIn0=",
                      "image_versions2": {
                          "candidates": []
                      },
                      "original_width": 612,
                      "original_height": 612,
                      "video_versions": [],
                      "like_count": 38544,
                      "has_liked": false,
                      "like_and_view_counts_disabled": false,
                      "can_viewer_reshare": true,
                      "integrity_review_decision": "pending",
                      "top_likers": [],
                      "user": {
                          "pk": 95561,
                          "pk_id": "95561",
                          "username": "mosseri",
                          "full_name": "Adam Mosseri",
                          "is_private": false,
                          "is_verified": true,
                          "profile_pic_id": "3090458139926225297_95561",
                          "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/343392897_618515990300243_8088199406170073086_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=1&_nc_ohc=HX3vm4L7D44AX_09srG&edm=AMl8KrABAAAA&ccb=7-5&oh=00_AfBQ5qRqYCwxeopGHTpXx0LsYGnxpWEapsnHJpdkFruvEA&oe=64B4168D&_nc_sid=cd9b4f",
                          "friendship_status": {
                              "following": false,
                              "followed_by": false,
                              "blocking": false,
                              "muting": false,
                              "is_private": false,
                              "incoming_request": false,
                              "outgoing_request": false,
                              "text_post_app_pre_following": false,
                              "is_bestie": false,
                              "is_restricted": false,
                              "is_feed_favorite": false,
                              "is_eligible_to_subscribe": false
                          },
                          "has_anonymous_profile_picture": false,
                          "has_onboarded_to_text_post_app": true,
                          "account_badges": []
                      }
                  }
              }
          },
          "integrity_review_decision": "pending",
          "ig_media_sharing_disabled": false,
          "has_shared_to_fb": 0,
          "is_unified_video": false,
          "should_request_ads": false,
          "is_visual_reply_commenter_notice_enabled": true,
          "commerciality_status": "not_commercial",
          "explore_hide_comments": false,
          "has_delayed_metadata": false
      },
      "upload_id": "1689241970",
      "status": "ok"
  }
  ```
</details>

<details>
  <summary>Open UI example</summary>

  ![](./assets/examples/quote-thread.png)
</details>

#### Feed

##### Get recommended users

`threads.private_api.get_recommended_users` — get recommended users. Supports [pagination](#pagination), but has 
different response keys: `paging_token` instead of `next_max_id` as an `offset` and `has_more` as a value to detect
whether to stop iterations or not.

| Parameters |  Type   | Required | Restrictions | Description                                                  |
|:----------:|:-------:|:--------:|:------------:|--------------------------------------------------------------|
|  `limit`   | Integer |    No    |     `>0`     | A number of recommended users to get. Default value is `15`. |
|  `offset`  | Integer |    No    |      -       | A number of recommended users skip before fetching.          |

<details>
  <summary>Open code example</summary>
  
  ```python3
  >>> recommended_users = threads.private_api.get_recommended_users()
  >>> recommended_users
  {
      "users": [
          {
              "pk": 285464169,
              "pk_id": "285464169",
              "username": "sashachistova",
              "full_name": "Sasha Chistova",
              "account_badges": [],
              "profile_pic_url": "https://instagram.fiev6-1.fna.fbcdn.net/v/t51.2885-19/358049976_793858308785598_4202050856705005217_n.jpg?stp=dst-jpg_s150x150&_nc_ht=instagram.fiev6-1.fna.fbcdn.net&_nc_cat=106&_nc_ohc=sieozLj6SiAAX-AN0KH&edm=AHWpSgsBAAAA&ccb=7-5&oh=00_AfCMzBux6Y1Hzi0ZmBkD26hzer4ETDiRY8v6mfeGEbR6lw&oe=64B8BE6D&_nc_sid=ae9eda",
              "has_anonymous_profile_picture": false,
              "has_onboarded_to_text_post_app": true,
              "is_verified": true,
              "friendship_status": {
                  "following": false,
                  "followed_by": false,
                  "blocking": false,
                  "muting": false,
                  "is_private": false,
                  "incoming_request": false,
                  "outgoing_request": false,
                  "text_post_app_pre_following": false,
                  "is_bestie": false,
                  "is_restricted": false,
                  "is_feed_favorite": false,
                  "is_eligible_to_subscribe": false
              },
              "profile_context_facepile_users": [],
              "follower_count": 26459
          },
          ...
      ],
      "paging_token": "15",
      "has_more": true,
      "status": "ok"
  }
  ```
</details>
