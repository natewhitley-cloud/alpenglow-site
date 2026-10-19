# alpenglow-site

Static site for alpenglowsoftware.com, served by GitHub Pages from `main`.

## Rule: tag every App Store link

Every clickable link to `apps.shopify.com` on any page, new or edited, carries UTM tags so NOD's GA4 listing panel can show which page sent the visit:

```
?utm_source=alpenglowsoftware&utm_medium=<guide|homepage|page>&utm_campaign=<page-slug>
```

- `utm_source` is always `alpenglowsoftware` (NOD keys on it).
- `utm_medium` (lowercase): `guide` for pages under `guides/`, `homepage` for `index.html`, `page` for anything else. NOD shows "Your guide: <slug>", "Your page: <slug>" and "Your homepage".
- `utm_campaign`: the page's slug (usually its folder name), lowercase letters, digits and hyphens only, at most 60 characters. Anything else and NOD shows only "Your guides" / "Your pages".
- Optional `utm_content` for link position (`top`, `middle`, `bottom`).
- In HTML write `&amp;` between parameters.
- Leave JSON-LD `url` / `sameAs` untagged; they identify the app, they aren't clicks.

Checks before committing (both must print nothing):

```
grep -rhoI 'href="https://apps\.shopify\.com/[^"]*"' --include='*.html' . | grep -v utm_source=alpenglowsoftware
grep -rhoI 'href="https://apps\.shopify\.com/[^"]*"' --include='*.html' . | grep -vE 'utm_medium=(guide|homepage|page)&amp;utm_campaign=[a-z0-9-]{1,60}("|&amp;)'
```

They only see double-quoted `href="https://apps.shopify.com/..."` links; write links that way.
