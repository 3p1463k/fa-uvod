# První kroky

Nejjednodušší soubor FastAPI by mohl vypadat takto:
```Python
{!../docs_src/first_steps/tutorial001.py!}
```

Zkopírujte to do souboru `main.py`.

Spusťte živý server:
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

!!! note
    Příkaz `uvicorn main:app` odkazuje na:

    * `main`: soubor `main.py` (Python "module").
    * `app`: objekt vytvořený uvnitř `main.py` na řádku s `app = FastAPI()`.
    * `--reload`: po změně kódu restartujte server. Používejte pouze pro vývoj.

Ve výstupu je řádek s něčím jako:

```hl_lines="4"
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

Tento řádek zobrazuje adresu URL, na které se vaše aplikace zobrazuje na vašem místním počítači.

### Vyzkoušej to

Otevřete prohlížeč na adrese <a href="http://127.0.0.1:8000" class="external-link" target="_blank">http://127.0.0.1:8000</a>.

Uvidíte odezvu JSON jako:
```JSON
{"message": "Hello World"}
```

### Interaktivní  API dokumentace

Nyní přejděte na <a href="http://127.0.0.1:8000/docs" class="external-link" target="_blank">http://127.0.0.1:8000/docs</a>.

Zobrazí se automatická interaktivní dokumentace API (provided by <a href="https://github.com/swagger-api/swagger-ui" class="external-link" target="_blank">Swagger UI</a>):

![Swagger UI](https://fastapi.tiangolo.com/img/index/index-01-swagger-ui-simple.png)

### Alternativní dokumentace API

A teď jděte na <a href="http://127.0.0.1:8000/redoc" class="external-link" target="_blank">http://127.0.0.1:8000/redoc</a>.

Zobrazí se alternativní automatická dokumentace (provided by <a href="https://github.com/Rebilly/ReDoc" class="external-link" target="_blank">ReDoc</a>):

![ReDoc](https://fastapi.tiangolo.com/img/index/index-02-redoc-simple.png)

### OpenAPI

**FastAPI** vygeneruje "schéma" se všemi vašimi API pomocí **OpenAPI** (standard pro definování API).

#### "Schema"

"Schéma" je definice nebo popis něčeho. Ne kód, který jej implementuje, ale pouze abstraktní popis.

#### API "schema"

V tomto případě, <a href="https://github.com/OAI/OpenAPI-Specification" class="external-link" target="_blank">OpenAPI</a> je specifikace, která určuje, jak definovat schéma vašich API.

Tato definice schématu zahrnuje cesty API, možné parametry, které přijímají, atd.

#### Data "schema"

Termín "schéma" může také odkazovat na tvar některých dat, jako je obsah JSON.
V tomto případě by to znamenalo atributy JSON a datové typy, které mají, atd.

#### OpenAPI a JSON Schema

OpenAPI definuje API schéma pro vaše API. A toto schéma zahrnuje definice (nebo "schemata") dat odeslaných a přijatých vaším API pomocí **JSON Schema**, standard pro datová schémata JSON.

#### Více o `openapi.json`

Pokud vás zajímá, jak vypadá surové schéma OpenAPI, FastAPI automaticky generuje JSON (schéma) s popisy všech vašich API.

Můžete to vidět přímo na: <a href="http://127.0.0.1:8000/openapi.json" class="external-link" target="_blank">http://127.0.0.1:8000/openapi.json</a>.

Ukáže to JSON začínající něčím jako:
```JSON
{
    "openapi": "3.0.2",
    "info": {
        "title": "FastAPI",
        "version": "0.1.0"
    },
    "paths": {
        "/items/": {
            "get": {
                "responses": {
                    "200": {
                        "description": "Successful Response",
                        "content": {
                            "application/json": {}}}}}}}}



```

#### K čemu slouží OpenAPI

Schéma OpenAPI je to, co pohání dva zahrnuté interaktivní dokumentační systémy.

A existují desítky alternativ, všechny založené na OpenAPI. Do své aplikace můžete snadno přidat kteroukoli z těchto alternativ vytvořenou s **FastAPI**.

Můžete jej také použít k automatickému generování kódu pro klienty, kteří komunikují s vaším API. Například frontendové, mobilní nebo IoT aplikace.

## Shrnutí, krok za krokem

### Krok 1: import `FastAPI`

```Python hl_lines="1"
{!../docs_src/first_steps/tutorial001.py!}
```

`FastAPI`je třída Pythonu, která poskytuje všechny funkce pro vaše API.

!!! note "Technické údaje"
    `FastAPI` je třída, která přímo dědí od `Starlette`.

    Můžete použít všechny funkce <a href="https://www.starlette.io/" class="external-link" target="_blank">Starlette</a>  s `FastAPI`.

### Krok 2: vytvořit `FastAPI` "instanci"

```Python hl_lines="3"
{!../docs_src/first_steps/tutorial001.py!}
```

Zde bude proměnná `app` "instancí" třídy `FastAPI`.

Toto bude hlavní bod interakce pro vytvoření všech vašich API.

Tato „app“ je stejná, jakou uvádí „uvicorn“ v příkazu:

<div class="termy">

```console
$ uvicorn main:app --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

</div>

Pokud svou aplikaci vytvoříte takto:
```Python hl_lines="3"
{!../docs_src/first_steps/tutorial002.py!}
```

A vložíte do souboru `main.py`, pak byste `uvicorn` spustili takto:
<div class="termy">

```console
$ uvicorn main:my_awesome_api --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

</div>

### Krok 3: vytvořit *path operation*

#### Cesta

"Cesta" zde odkazuje na poslední část adresy URL začínající prvním `/`.

Takže v adrese URL jako:
```
https://example.com/items/foo
```

... by cesta byla:
```
/items/foo
```

!!! info
    „Cesta“ se také běžně nazývá „koncový bod“ (endpoint), nebo „trasa“ (route).

Při vytváření API je „cesta“ hlavním způsobem, jak oddělit „zodpovědnosti“ a „zdroje“.

#### Operace/Metoda

„Operace“ zde odkazuje na jednu z „metod HTTP“.

Jednu z:

* `POST`
* `GET`
* `PUT`
* `DELETE`

...a ty exotičtější:

* `OPTIONS`
* `HEAD`
* `PATCH`
* `TRACE`

V protokolu HTTP můžete komunikovat s každou cestou pomocí jedné (nebo více) z těchto „metod“.

---

Při vytváření rozhraní API obvykle používáte tyto specifické metody HTTP k provedení konkrétní akce.

Normálně používáte:

* `POST`: k vytvoření dat.
* `GET`: ke čtení dat.
* `PUT`: k aktualizaci dat.
* `DELETE`: ke smazání dat.

V OpenAPI se tedy každá z metod HTTP nazývá „operation“.

Budeme  jim tedy také říkat „**operation**“.

#### Definujeme  *path operation dekorátor*

```Python hl_lines="6"
{!../docs_src/first_steps/tutorial001.py!}
```

`@app.get("/")` říká **FastAPI** že funkce pod dekorátorem má na starosti zpracování požadavků, které jdou na:

* cestu (path) `/`
* použitím <abbr title="an HTTP GET method"><code>get</code> operation</abbr>

!!! info "`@dekorátor` Info"
     `@něco` syntaxe v Pythonu se nazývá "dekorátor".

    Dáte to nad funkci. Jako pěkný dekorativní klobouk (Myslím, že odtud ten termín vznikl).

"Dekorátor" převezme funkci níže a něco s ní udělá.
    V našem případě tento dekorátor říká **FastAPI** že funkce níže koresponduje k **path** `/` s **operation** `get`.

Je to "**dekorátor operace cesty**".

Můžete také použít další operace:

* `@app.post()`
* `@app.put()`
* `@app.delete()`

A ty exotičtější:

* `@app.options()`
* `@app.head()`
* `@app.patch()`
* `@app.trace()`

!!! tip
    Každou operaci (metodu HTTP) můžete volně používat, jak chcete.
    **FastAPI** nevynucuje žádný konkrétní význam.
    Zde uvedené informace jsou uvedeny jako vodítko, nikoli jako požadavek.
    Například, když používáte GraphQL, normálně provádíte všechny akce pouze pomocí operací `POST`.

### Krok 4: definuj **path operation function**

Toto je naše „**funkce operace cesty**“:

* **cesta (path)**: je `/`.
* **operace (operation)**: je `get`.
* **funkce**: je funkce pod "dekorátorem" (`@app.get("/")`).

```Python hl_lines="7"
{!../docs_src/first_steps/tutorial001.py!}
```

Toto je funkce Pythonu.

Bude volána **FastAPI**, kdykoli obdržíme požadavek na URL "`/`" pomocí operace `GET`.

V tomto případě se jedná o `asynchronní` funkci.

---

Můžete ji také definovat jako normální funkci namísto `async def`:

```Python hl_lines="7"
{!../docs_src/first_steps/tutorial003.py!}
```

!!! note
    Pokud neznáte rozdíl, podívejte se na [Async: *"In a hurry?"*](../async.md){.internal-link target=_blank}.

### Krok 5: vrátit obsah

```Python hl_lines="8"
{!../docs_src/first_steps/tutorial001.py!}
```

Můžete vrátit `dict`, `list`, singulární hodnoty jako `str`, `int` atd.

Můžete také vrátit Pydantic modely (více o tom později).

Existuje mnoho dalších objektů a modelů, které budou automaticky převedeny na JSON (včetně ORM atd.). Zkuste použít své oblíbené, je vysoce pravděpodobné, že jsou již podporovány.

## Shrnutí

* Import `FastAPI`.
* Vytvořte `app` instanci.
* Napište **path operation dekorátor** (like `@app.get("/")`).
* Napište **path operation funkci** (like `def root(): ...`).
* Spusťte vývojový server ( `uvicorn main:app --reload`).
