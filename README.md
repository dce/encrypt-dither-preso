# Encrypt and Dither Photos in Hugo

David Eisinger  
Dev Meeting, 2024-02-13

---

## 1. Intro

---

## 2. Encrypt all images

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

## 3. Build a tiny image server

* [dither.rb](https://github.com/dce/davideisinger.com/blob/main/bin/dither/dither.rb)

---

## 4. Create a Hugo shortcode

---

## 5. Tweak site styles

---

## 6. Update deploy workflow

---

## 7. Share your work

---

## 8. Conclusion
