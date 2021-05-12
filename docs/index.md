# Guide to improve web-vitals

### FCP

#### What?

FCP measures how long it takes the browser to render the first piece of DOM content after a user navigates to your page. Images, non-white `canvas` elements, and SVGs on your page are considered DOM content; anything inside an iframe /isn’t/ included.

#### Theory

- Lazy-load third-party resources [Efficiently load third-party JavaScript](https://web.dev/efficiently-load-third-party-javascript/#lazy-load-third-party-resources)
- Establish early connections to required origins using `preconnect` `dns-prefetch`
  [Efficiently load third-party JavaScript](https://web.dev/efficiently-load-third-party-javascript/#lazy-load-third-party-resources)
- Use a “windowing” library like [react-window](https://web.dev/virtualize-long-lists-react-window/) to minimize the number of DOM nodes created if you are rendering many repeated elements on the page.
- Minimize unnecessary re-renders using [shouldComponentUpdate](https://reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action) , [PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent) , or [React.memo](https://reactjs.org/docs/react-api.html#reactmemo) .
- [Skip effects](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) only until certain dependencies have changed if you are using the Effect hook to improve runtime performance.
- [Avoid an excessive DOM size](https://web.dev/dom-size/#react)

#### Fail-fast principle

* Use a “windowing” library specially when rendering tons of cards above the fold.

* Use infinite scrolling alongside a windowing technique.

* Font-awesome and third-party resources should be self-hosted when possible to avoid DNS lookup and round-trip times.

* Use service-workers to cache scripts from third-party servers
[Handle Third Party Requests | Workbox | Google Developers](https://developers.google.com/web/tools/workbox/guides/handle-third-party-requests)

* Use image on load hook, https://usehooks-typescript.com/react-hook/use-image-on-load. Practical use-case, https://react-gallery-ux.netlify.app/
### LCP

#### What?

Largest Contentful Paint marks the time at which the largest text or image is painted.

#### Fail-fast principle

- Use next/image component with a custom loader to call our lambda
  loader: [next/image | Next.js](https://nextjs.org/docs/api-reference/next/image#loader)
  lambda: https://github.com/thebyte9/image-cdn-utils/blob/master/packages/image-cdn-utils/README.md
- Create a new admin attribute to set `priority` attribute to images above the fold to be preloaded.

  - [next/image | Next.js](https://nextjs.org/docs/api-reference/next/image#priority)

- Use intersection-observer to lazy load images above the fold.

### CLS (ad-related layout shift)

#### What?

Measures layout shifts that were caused by ads or happened near ads. Reducing cumulative ad-related layout shift will improve user experience. [Learn more](https://developers.google.com/publisher-ads-audits/reference/audits/cumulative-ad-shift?utm_source=lighthouse&utm_medium=devtools) .

#### Theory

[Optimize Cumulative Layout Shift](https://web.dev/optimize-cls/)

Note: exactly above is what we should improve in Dockwalk and BoatInternational.

#### Fail-fast principle

CLS score could be improved by reserving space for Multi-size ad slots, [Minimize layout shift | Google Publisher Tag | Google Developers](https://developers.google.com/publisher-tag/guides/minimize-layout-shift#multisize)

### CLS (not ad-related)

#### What?

Measures ad-related content layout shifts

#### Fail-fast principle

Build skeleton components and render skeleton screens to avoid any layout shifts and comply with business constraints such as multiple add sizes.

We don’t need to use any external react library. It could be accomplished by just using CSS, e.g [Animation - Tailwind CSS](https://tailwindcss.com/docs/animation#pulse)

### Speed Index

#### What?

Speed Index measures how quickly content is visually displayed during page load.

#### Theory

- [Minimize main thread work](https://web.dev/mainthread-work-breakdown)
- [Reduce JavaScript execution time](https://web.dev/bootup-time)
- [Ensure text remains visible during webfont load](https://web.dev/font-display)

#### Fail-fast principle

- Defer and eliminate render-blocking resources.
- [Eliminate render-blocking resources](https://web.dev/render-blocking-resources/)
- Use web workers to minimise main thread work as execution takes place in different threads.

### Eliminate unused Javascript and CSS on main page

[Find Unused JavaScript And CSS With The Coverage Tab - Chrome Developers](https://developer.chrome.com/docs/devtools/coverage/)

## Fail-fast principle in software development

https://dzone.com/articles/fail-fast-principle-in-software-development
