# Encrypt and Dither Photos in Hugo

David Eisinger  
Dev Meeting, 2024-02-13

---

## 1. Intro

---

## 2. Encrypt all images

* [aes-256-cbc](https://www.bjornjohansen.com/encrypt-file-using-ssh-key)

---

## 2. Encrypt all images

Make a key:

```sh
openssl rand -hex -out secret.key 32
```

```sh
echo secret.key >> .gitignore  
```

---

## 2. Encrypt all images

Encrypt a file:

```sh
echo "hello!" > test.txt
```

```sh
openssl \
  aes-256-cbc \
  -in test.txt \
  -out test.txt.enc \
  -pass file:secret.key \
  -iter 1000000
```

---

## 2. Encrypt all images

Decrypt a file:

```sh
rm test.txt
```

```sh
openssl \
  aes-256-cbc \
  -d \
  -in test.txt.enc \
  -out test.txt \
  -pass file:secret.key \
  -iter 1000000
```

---

## 2. Encrypt all images

```ruby
Dir.glob("content/**/*.{jpg,jpeg,png}").each do |path|
  %x(
    openssl \
      aes-256-cbc \
      -in #{path} \
      -out #{path}.enc \
      -pass file:secret.key \
      -iter 1000000
  )
end
```

---

## 3. Build a tiny image server

* [Sinatra](https://sinatrarb.com/)
* [MiniMagick](https://github.com/minimagick/minimagick)
* [dither.rb](https://github.com/dce/davideisinger.com/blob/main/bin/dither/dither.rb)

---

## 3. Build a tiny image server

* <http://localhost:4567/journal/dispatch-12-february-2024/IMG_2374.jpeg?geo=800x600>

---

## 4. Create a Hugo shortcode

Allow `DITHER_SERVER` environment variable:

```toml
[security.funcs]
getenv = ['DITHER_SERVER']
```

---

## 4. Create a Hugo shortcode

Start Hugo:

```sh
DITHER_SERVER=http://localhost:4567 hugo server
```

---

## 4. Create a Hugo shortcode

* [dither.html](https://github.com/dce/davideisinger.com/blob/main/themes/v2/layouts/shortcodes/dither.html)
* [Example](https://github.com/dce/davideisinger.com/blob/main/content/journal/dispatch-12-february-2024/index.md?plain=1#L84-L85)

---

## 5. Delete unencrypted images

* [git filter-repo](https://github.com/newren/git-filter-repo)
* <https://stackoverflow.com/a/64563565>

---

## 5. Delete unencrypted images

```ruby
Dir.glob("content/**/*.{jpg,jpeg,png}") do |path|
  `git filter-repo --invert-paths --force --path #{path}`
end
```

---

## 6. Tweak site styles

---

## 6. Tweak site styles

```css
img {
  mix-blend-mode: multiply;
}
```

* <https://davideisinger.com/journal/dispatch-12-february-2024/>

---

## 7. Update deploy workflow

* [deploy.yml](https://github.com/dce/davideisinger.com/blob/main/.github/workflows/deploy.yml)

---

## 8. Share your work

* <https://davideisinger.com/journal/encrypt-and-dither-photos-in-hugo/>
* <https://discourse.gohugo.io/t/encrypt-and-dither-photos-in-hugo/48157>
* <https://github.com/gohugoio/hugo/pull/12016>

---

## 9. Conclusion

* Hugo's pretty neat
* Build small things
* Share your work
