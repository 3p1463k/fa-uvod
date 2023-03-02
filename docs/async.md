# Konkurence  async / await

Podrobnosti o syntaxi `async def` pro *funkce operace cesty* a některé pozadí o asynchronním kódu, souběžnosti a paralelismu.

## Ve spěchu?

<abbr title="too long; didn't read"><strong>TL;DR:</strong></abbr>

Pokud používáte knihovny třetích stran, které vám říkají, abyste je zavolali pomocí `await`, například:

```Python
results = await some_library()
```

Poté deklarujte své *funkce operace cesty* pomocí `async def` jako:

```Python hl_lines="2"
@app.get('/')
async def read_results():
    results = await some_library()
    return results
```

!!! note
    `await` můžete použít pouze uvnitř funkcí vytvořených pomocí `async def`.
---

Pokud používáte knihovnu třetí strany, která s něčím komunikuje (databází, rozhraním API, souborovým systémem atd.) a nemá podporu pro použití `await`, (to je v současnosti případ většiny databázových knihoven), pak deklarujte své *funkce operace cesty* jako normálně, pouze s `def`, jako:

```Python hl_lines="2"
@app.get('/')
def results():
    results = some_library()
    return results
```

---

Pokud vaše aplikace (nějak) nemusí komunikovat s ničím jiným a čekat, až zareaguje, použijte `async def`.

---

Pokud si nejste jisti, použijte normální `def`.

---

**Poznámka**: Můžete kombinovat `def` a `async def` ve svých *funkcích operace cesty* jakkoli potřebujete, a každou z nich definovat pomocí nejvhodnější možnosti. FastAPI s nimi udělá správnou věc.

Každopádně v kterémkoli z výše uvedených případů bude FastAPI stále fungovat asynchronně a bude extrémně rychlé.

Ale podle výše uvedených kroků bude schopna provést určité optimalizace výkonu.

## Technické detaily

Moderní verze Pythonu mají podporu pro **"asynchronní kód"** pomocí něčeho, co se nazývá **"coroutines"**, se syntaxí **`async` a `await`**.

Podívejme se na tuto frázi po částech v následujících částech:

* **Asynchronní kód**
* **`async` and `await`**
* **Coroutines**

## Asynchronní kód

Asynchronní kód jen znamená, že jazyk má způsob, jak sdělit počítači/programu, že v určitém bodě kódu bude muset 🤖 počkat, až *něco-jiného* skončí někde jinde. Řekněme, že *něčemu jinému* se říká "pomalý-soubor".

Takže během té doby může počítač jít dělat nějakou jinou práci, zatímco „pomalý soubor“ skončí.
Potom se počítač / program vrátí pokaždé, když bude mít příležitost, protože znovu čeká, nebo kdykoli dokončí všechnu práci, kterou v tu chvíli měl.A uvidí, jestli některý z úkolů, na který čekal, už skončil, a udělal, co udělat měl.

Dále vezme první úkol k dokončení (řekněme náš „pomalý-soubor“ ) a pokračuje ve všem, co s tím mělo společného.

To "počkej na něco jiného" normálně odkazuje <abbr title="Input and Output">I/O</abbr> operace, které jsou relativně „pomalé“ (ve srovnání s rychlostí procesoru a paměti RAM), jako je čekání na:

* data od klienta, která mají být odeslána přes síť
* data odeslaná vaším programem, která mají být přijata klientem prostřednictvím sítě
* obsah souboru na disku, který má být přečten systémem a předán vašemu programu
* obsah, který váš program dal systému k zápisu na disk
* vzdálená API operace
* databázová operace k dokončení
* databázový dotaz pro vrácení výsledků
* atd.

Protože doba realizace je spotřebována většinou čekáním <abbr title="Input and Output">I/O</abbr> operace, nazývají je operacemi „I/O bound“.

Říká se tomu „asynchronní“, protože počítač/program nemusí být „synchronizován“ s pomalou úlohou, čekat na přesný okamžik, kdy úloha skončí, a přitom nic nedělat, aby mohl vzít výsledek úlohy a pokračovat v práci. .

Namísto toho, protože je to „asynchronní“ systém, po dokončení může úkol chvíli čekat ve frontě (několik mikrosekund), než počítač / program dokončí, co udělal, a pak se vrátit a vzít si výsledky a s nimi dále pracovat.

Pro „synchronní“ (na rozdíl od „asynchronního“) se běžně také používá“ termín „sekvenční“, protože počítač/program následuje všechny kroky v pořadí před přepnutím na jinou úlohu, i když tyto kroky zahrnují čekání.

Díky tomuto druhu asynchronicity je NodeJS populární (i když NodeJS není paralelní) a to je síla Go jako programovacího jazyka.

A to je stejná úroveň výkonu, jakou získáte s **FastAPI**.

A protože můžete mít paralelismus a asynchronitu zároveň, získáte vyšší výkon než většina testovaných rámců NodeJS a srovnatelný s Go, což je kompilovaný jazyk blíže C <a href="https://www.techempower.com/benchmarks/#section=data-r17&hw=ph&test=query&l=zijmkf-1" class="external-link" target="_blank">(to vše díky Starlette)</a>.

### Je souběžnost lepší než paralelismus?

Ani náhodou! To není morálka příběhu.

Souběžnost je něco jiného než paralelismus. A je to lepší na **specifických** scénářích, které vyžadují hodně čekání. Z tohoto důvodu je pro vývoj webových aplikací obecně mnohem lepší než paralelismus.Ale ne na všechno.

Abychom to vyvážili, představte si následující krátký příběh:

> Musíte uklidit velký, špinavý dům.

*Jo, to je celý příběh*.

---

Na nic se nečeká 🕙, vše je k dispozici, jen je třeba udělat spoustu práce na více místech v domě.

Mohli byste střídat místnosti , nejdřív obývák, pak kuchyň, ale jelikož na nic nečekáte 🕙, úklid je úklid,  střídat místnosti by nic neovlivnilo.

Dokončení se střídáním nebo bez něj by trvalo stejně dlouho (souběh) a udělali byste stejné množství práce.

Ale v  případě, kdybyste mohli přivést 8 přátel, a každý z nich (plus vy) by si mohl vzít zónu domu, aby ho uklízel, veškerou práci byste mohli dělat **paralelně** s extra pomocí a skončit mnohem dříve.

V tomto scénáři by každý z čističů (včetně vás) byl procesor, který by dělal svou část práce.

A protože většinu času provádění zabírá skutečná práce (místo čekání) a práci v počítači provádí <abbr title="Central Processing Unit">CPU</abbr>, nazývají se tyto problémy "CPU bound".

---

Běžné příklady operací vázaných na CPU jsou věci, které vyžadují složité matematické zpracování.

Například:

* **Audio** nebo **image processing**.
* **Computer vision** obrázek se skládá z milionů pixelů, každý pixel má 3 hodnoty / barvy, zpracování normálně vyžaduje něco na těchto pixelech vypočítat a to vše současně.

* **Strojové učení**: vyžaduje spoustu "maticových" a "vektorových" násobení. Představte si obrovskou tabulku s čísly a vynásobte je všechny najednou.

* **Deep Learning**: toto je podobor strojového učení, takže platí totéž. Jde jen o to, že neexistuje jen jediná tabulka čísel k násobení, ale jejich obrovská sada a v mnoha případech k sestavení a/nebo použití těchto modelů používáte speciální procesor.


## `async` and `await`

Moderní verze Pythonu mají velmi intuitivní způsob, jak definovat asynchronní kód. Díky tomu to vypadá jako normální „sekvenční“ kód co „počká“ ve správných okamžicích.

Když dojde k operaci, která bude vyžadovat čekání před poskytnutím výsledků a má podporu pro tyto nové funkce Pythonu, můžete ji nakódovat takto:

```Python
burgers = await get_burgers(2)
```

Klíčem je zde `await`. Říká Pythonu, že musí počkat ⏸ na `get_burgers(2)` dokončit svou věc 🕙 před uložením výsledků do `burgers`. Díky tomu Python bude vědět, že může jít a mezitím udělat něco jiného 🔀 ⏯ (například přijmout další požadavek).

Aby `await` fungovalo, musí být uvnitř funkce, která tuto asynchronitu podporuje. Chcete-li to provést, stačí jej deklarovat pomocí `async def`:

```Python hl_lines="1"
async def get_burgers(number: int):
    # Do some asynchronous stuff to create the burgers
    return burgers
```

...místo `def`:

```Python hl_lines="2"
# This is not asynchronous
def get_sequential_burgers(number: int):
    # Do some sequential stuff to create the burgers
    return burgers
```

S `async def` Python ví, že uvnitř této funkce si musí být vědom výrazů `await` a že může „pozastavit“ ⏸ provádění této funkce a jít dělat něco jiného 🔀, než se vrátí.
Když chcete volat funkci `async def`, musíte na ni „čekat“. Tohle nebude fungovat:

```Python
# Toto nebude fungovat, protože get_burgers byl definován pomocí: async def
burgers = get_burgers(2)
```

---

Pokud tedy používáte knihovnu, která vám říká, že ji můžete volat pomocí `await`, musíte vytvořit *funkce operace cesty*, které ji používají s `async def`, jako v:

```Python hl_lines="2-3"
@app.get('/burgers')
async def read_burgers():
    burgers = await get_burgers(2)
    return burgers
```

### Další technické detaily

Možná jste si všimli, že `await` lze použít pouze uvnitř funkcí definovaných pomocí `async def`.

Zároveň však musí být funkce definované pomocí `async def` "očekávány" ("awaited").Takže funkce s `async def` lze volat pouze uvnitř funkcí definovaných pomocí `async def`.

Takže, pokud jde o vejce nebo kuře, jak zavoláte první funkci `async`?

Pokud pracujete s **FastAPI**, nemusíte se o to starat, protože tato „první“ funkce bude vaše *funkce operace cesty* a FastAPI bude vědět, jak udělat správnou věc.


## Coroutines

**Coroutine** je jen  termín pro věc vrácenou funkcí `async def`. Python ví, že je to něco jako funkce, kterou může spustit a že v určitém okamžiku skončí, ale že může být pozastavena ⏸ i interně, kdykoli je "await" uvnitř.

Ale veškerá tato funkčnost používání asynchronního kódu s `async` a `await` je mnohokrát shrnuta jako použití "coroutines". Je srovnatelný s hlavním klíčovým prvkem Go, „Goroutines“.

## Závěr

Podívejme se na zmíněnou frázi:

> Moderní verze Pythonu mají podporu pro **"asynchronní kód"** pomocí něčeho, co se nazývá **"coroutines"**, se syntaxí **`async` a `await`**.

To by teď mělo dávat větší smysl.

To vše pohání FastAPI (prostřednictvím Starlette) a díky tomu má tak působivý výkon.
