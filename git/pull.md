Давайте рассмотрим, как `fetch` и `merge` выполняются последовательно
![](Pasted%20image%2020230609152659.png)
```bash
git fetch; git merge o/main
```
![](Pasted%20image%2020230609152736.png)
Опа - мы скачали `C3` с помощью команды `fetch` и затем объединяем эти наработки с помощью `git merge o/main`. Теперь наша ветка `main` отображает изменения с удалённого репозитория (в данном случае — с репозитория `origin`)

---

Что же произойдёт, если вместо этих команд мы воспользуемся `git pull`?
![](Pasted%20image%2020230609152840.png)
```bash
git pull
```
![](Pasted%20image%2020230609152939.png)

---
### `pull --rebase`
![](Pasted%20image%2020230609154537.png)
```bash
git pull --rebase; git push
```
![](Pasted%20image%2020230609154621.png)

---

### Обычный `pull`
![](Pasted%20image%2020230609154707.png)
```bash
git pull; git push
```
![](Pasted%20image%2020230609154746.png)