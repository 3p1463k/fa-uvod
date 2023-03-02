# Uživatelská příručka

Tento tutoriál vám krok za krokem ukáže, jak používat FastAPI a většinu jeho funkcí.

Každý díl postupně navazuje na předchozí, ale je strukturován do samostatných témat, takže můžete přejít přímo na kterékoli konkrétní téma a vyřešit vaše specifické potřeby API.

Je také postaven tak, aby fungoval jako budoucí reference.

Můžete se tedy vrátit a nalézt přesně to, co potřebujete.


## Instalace

Prvním krokem je instalace FastAPI.

Pro tutorial jej možná budete chtít nainstalovat se všemi volitelnými závislostmi a funkcemi:
<div class="termy">

```console
$ pip install "fastapi[all]"

---> 100%
```

</div>

... obsahuje také `uvicorn`, který můžete použít jako server, na kterém běží váš kód.


!!! note
Můžete také nainstalovat po částech.
Toto byste pravděpodobně udělali, jakmile budete chtít nasadit aplikaci do produkce:
    ```
    pip install fastapi
    ```

Nainstalujte také `uvicorn`, aby fungoval jako server:
    ```
    pip install "uvicorn[standard]"
    ```

A totéž pro každou z volitelných závislostí, které chcete použít.

---

## Spouštění kódu

Všechny bloky kódu lze kopírovat a používat přímo (jsou to vlastně testované soubory Pythonu).
Chcete-li spustit některý z příkladů (v další sekci), zkopírujte kód do souboru `main.py` a začněte `uvicorn` takto:
<div class="termy">

```console
$ uvicorn main:app --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
<span style="color: green;">INFO</span>:     Started reloader process [28720]
<span style="color: green;">INFO</span>:     Started server process [28722]
<span style="color: green;">INFO</span>:     Waiting for application startup.
<span style="color: green;">INFO</span>:     Application startup complete.
```

</div>

Je **doporučeno**, abyste kód napsali nebo zkopírovali, upravili a spustili lokálně.

Jeho použití ve vašem editoru je to, co vám skutečně ukáže výhody FastAPI, uvidíte, jak málo kódu musíte napsat, všechny kontroly typu, automatické dokončování atd.
