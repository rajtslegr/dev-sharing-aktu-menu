---
theme: default
highlighter: shiki
lineNumbers: true
info: >
  ## Micro Frontend Architecture or how we built a new menu for Aktualne.cz by
  Petr Rajtslegr
drawings:
  persist: false
css: unocss
title: Micro Frontend Architecture
---

# Micro Frontend Architecture

<p class="opacity-50">
... or how we built a new navigation bar for Aktualne.cz

Petr Rajtslegr
</p>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/rajtslegr" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
Případová studie 
- jak to vlastně začlo? 
- od produktu přišlo zadání, udělejte nové menu
- máme k dispozici návrh - figma
-->

---

# Micro Frontend Architecture

<v-clicks>

- Split monolithic frontend codebase into smaller apps
- Autonomous teams
- Freedom to choose the tech stack
- Decoupled codebases
- Scalability
- Fault isolation
- Faster build time
- Independent deployment
- Reusability

</v-clicks>

<!--
Co to vlastně je micro fronted architekru a k čemu to je dobrý
- základní myšelenka - rozdělení monolitu do menších aplikací
- každou micro servicu může teda zpravovat autonomní tým
- máme decouplovanou codebase
- jednodušší škálování a iterace
- máme izolované chyby, neovlivňují zbytek systému
- zrychlujeme build, nemusíme buildit celou aplikaci
- k tomu se váže nezávislý deployment
- znovu použitelnost micro-service
-->

---

# Aktualne.cz - Desktop

<div class="wrap">
  <iframe src="https://aktualne.cz" class="h-124"></iframe>
</div>

<style>
  .wrap {
    overflow: hidden;
  }

  iframe {
    width: 200% !important;
    transform: scale(0.5);
    transform-origin: 0 0;
  }
</style>

<!-- 
- kdo neví o co jde, ukázka jak menu vypadá
- dneska už v produkci
- lišta
- otvírá se nějaká plachta
- vyhledávací input s animací
- svátky
- počasí (2h - refresh)

-->

---

# Aktualne.cz - Mobile

<div class="wrap">
  <iframe src="https://aktualne.cz" class="w-full h-62"></iframe>
</div>

<style>
  .wrap {
    overflow: hidden;
  }

  iframe {
    width: 100% !important;
    transform: scale(1);
    transform-origin: 0 0;
  }
</style>

<!--
- mobilní verze
- menu je samozřejmě responzivní
-->

---

# Problems

<v-clicks>

- BBX
- Bundle size
- CSS conflicts
- How (the hell) do we deploy it?

</v-clicks>

<!--
Co jsme se řešili za problémy?
- **CLICK**
- BBX, šifra mistra leonarda
- není to moc pěkný developer experience
- věděli jsme, že totožné menu budeme potřebovat i jinde (Blogy)
- **CLICK**
- frontend, bude to asi dost velké, dokážeme to vůbec udělat menší?
- **CLICK**
- konflikty v css
- globální resety
- obousměrné konflikty
- **CLICK**
- jak to vlastně sakra nasadíme?
- to bude asi to nejzajímavější..
-->

---

# Technologies

<v-clicks>

- <logos-vitejs /> **Vite**
- <logos-react /> **React**
- <logos-preact /> **Preact**
- <logos-tailwindcss-icon /> **Tailwind CSS**
- <logos-swr class="bg-gray rounded p-0.5" /> **SWR**
- <span class="w-[1.2em] h-[1.2em] mr-0.6 text-[#770c56]">(◷)</span>**date-fns**
- <logos-jest /> **Jest**
- <logos-netlify /> **Netlify**
- <logos-storybook-icon /> **Storybook**
- <logos-cloudflare-workers-icon /> **Cloudflare Workers**
- <logos-aws-s3 /> **AWS S3 bucket**
- <logos-github-actions /> **GitHub Actions**
- <logos-webhooks /> **Webhooks**

</v-clicks>

<!--
Co jsme použili za technologie, dál si řekneme víc...
- **CLICK**
- Bundler, alternativa k CRA
- používá rollup
- od VUE core týmu, velká podpora open-source komunity
- Vanila JS, React, Vue, Svelte nebo třeba lit
- super DX, hlavně HMR
- plně natypováno, TS out-of-the-box
- **CLICK**
- náš weapon of choice
- asi není co dodávat
- **CLICK**
- ještě si řekneme víc
- **CLICK**
- známe z minulého dev-sharingu
- **CLICK**
- hooky pro data fetching
- proč sosání počasí
- **CLICK**
- alternative k moment.js (deprecated), doporučuju
- generování svá†ku
- **CLICK**
- unit testing
- jak FE, tak Cloudflare Worker
- **CLICK**
- PR preview, storybook
- není tam produkční build, který tahá bundle
- **CLICK**
- pomocí níž řešíme deployment a produkční bundle
-->

---

# Bundle size

```bash {1-4|5-8}
# Pre-optimization
10:06:33 AM: netlify-build/menu/assets/index.be022eb2.css   24.29 KiB / gzip: 4.38 KiB
10:06:33 AM: netlify-build/menu/assets/index.9af229a6.js    242.08 KiB / gzip: 77.44 KiB

# Post-optimization
1:52:00 PM: netlify-build/menu/assets/index.6d599999.css   24.61 KiB / gzip: 4.34 KiB
1:52:00 PM: netlify-build/menu/assets/index.93f4176f.js    80.11 KiB / gzip: 30.91 KiB
```

<!--
- snížení pomocí optimalizací a prací s dependencies
-->

---

# Preact

<v-click>

- React alternative (same API)
- Small size
- Big performance

</v-click>

<v-click>

```ts
// vite.config.ts
import preact from '@preact/preset-vite';
...

return {
   plugins: [
      preact(),
      ...
   ],
   ...
});   
```

</v-click>

<!--
- alternative k Reactu (stejné API)
- **CLICK**
- small bundle
- 3kB gzip
- **CLICK**
- big performance
- **CLICK**
- vyžvejkne maximum z prohlížeče
- **CLICK**
- plugin pro Vite
- nice neřešíme, nahradí importy
- pozor na kompatibilitu, není asi ideální pro větší projekty
- Uber, mobilní verze svojí web app 
-->

---

# SWR

<v-click>

- React Hooks library for data fetching

</v-click>

<v-click>

```ts
const useWeatherData = () =>
  useSWR<WeatherData>(
    'https://example.com/route/json',
    fetchFactory,
    {
      revalidateOnFocus: false,
      revalidateOnReconnect: false,
      refreshInterval: cacheTimeHours,
    },
  );
```

</v-click>

<v-click>

```tsx
const { error, data } = useWeatherData();
```

</v-click>

<!-- 
- hooka pro tahání počasí
- alternative k react-query, kterou jsme použili původně
- od Vercelu, primárně pro klientskou hydrataci Next.js
- je menší
- nemá sice všechny feature, ale nepotřebujeme (jednoduchost ideál)
-->

---

<iframe class="w-full h-full" src="https://asset.stdout.cz/aktu-menu/stats.html" />

<!-- 
- takhle aktuálně vypadá analýza/metrika bundle
-->

---

# Styling conflicts

<v-click>

- Importants, scoped base CSS/normalization, prefixes  

</v-click>

<v-click>

```ts
// tailwind.config.js
module.exports = {
  important: true,
  corePlugins: {
    preflight: false,
  },
  prefix: 'am-',
  ...
}
```

</v-click>

<v-click>

```tsx
// Menu.tsx
...
<header
  className={clsx(
    'am-sticky am-top-0 am-h-14 am-w-full am-bg-primary-blue-100 lg:am-h-16',
    className,
  )}
>
...
```

</v-click>

<!-- 
- Toho jsme se báli docela hodně
- naštěstí Tailwind pomocí konfigurace jednoduše umožňuje
1) zapnout importanty 
2) vypnout base/normalizer/resety
3) zapnout prefixy
- **CLICK**
- takhle nějak to vypadá s prefixy
-->

---

# Cloudflare Workers

<v-clicks>

- Serverless code
- Edge functions
- V8 JavaScript Engine
- Browser Service Worker API
- Personalization
- Authorization
- KV Storage

</v-clicks>

---

# Deployment

<v-click>

- Merge to main branch

</v-click>

<v-click>

- GitHub Actions -> AWS S3 bucket

</v-click>

<v-click>

```yaml
- name: Deploy files to AWS S3
  run: |
    aws s3 sync \
      ./netlify-build/menu \
      s3://${{ env.AWS_BUCKET }}/${{ env.AWS_BUCKET_PREFIX }} \
      --cache-control max-age=604800
```

</v-click>

<v-click>

- GitHub Actions -> Webhook --> Cloudflare Worker

</v-click>

<v-click>

```yaml
- name: Send deploy webhook to Cloudflare worker
  if: success()
  run: |
    curl https://example.com/route/webhook \
      -H 'X-Github-Deploy-Event: success'
```

</v-click>

<!-- 
- Povíme si něco k deploy
- **CLICK**
- všechno začíná mergem to main větve v repositáři menu
- pomocí github uploadneme build do s-strojky
- **CLICK**
- a pingneme si cloudflare worker, aby věděl, že máme novou verzi
-->

---

# Deployment

- Cloudflare Worker

<v-click>

```js {all|3|9}
// menu/deploy.js
export const deployMenu = async () => {
  const menuHtml = await fetchRemoteMenu();

  if (!menuHtml) {
    return new Response('Cannot fetch.', { status: 502 });
  }

  await AKTU_STORE.put(menuKvKey, menuHtml);

  return new Response('Ok', { status: 200 });
};
```

</v-click>

<!-- 
Co se děje v tom workeru
- **CLICK**
- když víme, že máme novou verzi, menu si fetchneme a hodíme do KV storage
- **CLICK**
-->


---

# Deployment

- Cloudflare Worker

```js {all|8-11|7,14}
// menu/deploy.js
export const fetchRemoteMenu = async () => {
  try {
    const response = await fetch(menuUrl, {
      signal: AbortSignal.timeout(menuFetchTimeout),
    });
    const html = await response.text();
    const isResponseOk =
      isStatusOk(response) &&
      isContentTypeText(response) &&
      isMenuIncluded(html);

    if (isResponseOk) {
      return html;
    }
  } catch (error) {
    console.error(error);
  }
};
```
<!-- 
Takhle vypadá fetch funkce
- **CLICK**
- pokud projdou checky (200, jestli je to text a jestli je v tom co přijde to menu (komentář v index.html))
- **CLICK**
- tak ho vrátíme
-->

---

# HTML Rewriter

<v-clicks>

```js {all|3-4|7|10 }
// menu/rewrite.js
export const rewriteMenu = (request, cookies, response) => {
  const shouldRewriteMenu =
    isContentTypeText(response) && isSupportedBrowser(request);

  if (shouldRewriteMenu) {
    return menuRewriter.transform(response);
  }

  return response;
};
```

```js
const menuRewriter = new HTMLRewriter().on(
  '.worker-menu-replace',
  new MenuElementHandler(),
);
```

```html
<div id="header" n:class="menu, worker-menu-replace">
  ...
  {include 'menu-legacy.latte'}
  ...
```

</v-clicks>

<!--
HTML Rewriter -  jak název napovídá pomůže nám modifikovat response
- **CLICK**
- checkenem si podle user-agenta jestli podporujeme browser
- tak ho vrátíme
- **CLICK**
- jinak response nemodifikujeme, zůstane legacy menu
-->

---
class: 'text-center'
---

#  Thank you for your attention!

<div class="flex-col items-center">
  <div class="flex items-center justify-around">
    <Tweet class="w-72" id="1141771853574225920" />
    <Tweet class="w-72" id="1132493678730252288" />
  </div>
</div>

Made with [sli.dev](Slidev)

<!-- 
Díky za pozornost, haha sranda
- sli.dev - overkill a over-engineered prezentační framework :D
-->
