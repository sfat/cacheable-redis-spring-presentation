---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shikiji
lineNumbers: false
info: |
  ## Cacheable Redis Spring Presentation
drawings:
  persist: false
transition: slide-left
title: Cacheable Redis Spring Presentation
mdc: true
---

# Cacheable Redis Spring Presentation

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# What are we going to learn today?

-  **@Cacheable** - annotation provided by Spring
-  **Redis** - how is works

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Cache annotations

- **@Cacheable**
- **@CacheEvict**
- **@CachePut**

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# @Cacheable

Define a cache for the list of users

```java
@Cacheable(value = "users-cache")
public List<User> getAllUsers() {
  return userRepository.findAll();
}
```

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# @CacheEvict - single entry

Evict cache

```java
@CacheEvict(value = "users-cache", key = "#id")
public User deleteUser(String id) {
  return userRepository.deleteById(id);
}
```

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# @CacheEvict - all entries

Evict cache

```java
@CacheEvict(value="users-cache",allEntries = true)
public List<User> updateUsers(List<User> users) {
  return userRepository.saveAll(users);
}
```

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# @CachePut

Update cache

```java
@CachePut(value = "users-cache", key = "#user.id")
public User updateUser(User user) {
    return userRepository.save(user);
}
```

<!--
Presenter note with **bold**, *italic*, and ~~striked~~ text.

Also, HTML elements are valid:
<div class="flex w-full">
  <span style="flex-grow: 1;">Left content</span>
  <span>Right content</span>
</div>
-->

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Redis

- distributed cache
- key value store

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Get started with Redis

* redis docker container
* redis-cli command

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Redis docker container

```yaml
version: '3.7'
services:
  redis:
    image: redis:latest
    hostname: redis
    container_name: redis
    restart: always
    command: sh -c "redis-server --save 20 1 --requirepass dev"
    ports:
      - "6379:6379"

```

And you run `docker compose up -d`

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---
# redis-cli

```shell
redis-cli -h localhost
# authenticate
localhost:6379> AUTH dev
OK
# get all keys
localhost:6379> KEYS *
 1) "users-cache::1"
# get value for cache key
localhost:6379> GET "users-cache::1"
```

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---
# Best practices and gotchas

- Invocation of cacheable methods from the same class
- evict vs put
- reusing the same cache, but with different response
- use cache keys usage whenever possible
