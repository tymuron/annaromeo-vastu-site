# annaromeovastu.com — Anna Romeo's Vastu site

The Vastu offering that lived at `annaromeo.design/main` on Tilda, lifted onto
its own domain with that page as the home page. Nothing here loads from Tilda.

```
site/                     the deployable site (Render publishes this folder)
  index.html              was /main
  vastu-diagnostika/                          |
  vastu-dizain-kontseptsiya-interera/         | the five services,
  podbor-novoi-nedvizhimosti-po-vastu/        | was /main/tproduct/<ids>-<slug>
  konsultatsiya-vastu-dlya-biznesa/           |
  konsultatsiya-po-vastu-dizainu-dlya-diza/   |
  mk-vastudesign/         the Vastu + Design masterclass
  classpaper/             the Classpaper interview
  course/                 the flagship "Васту-дизайн" course, synced from the
                          vastu-course-landing repo (125 MB, mostly video)
  404.html
  assets/                 only what these pages use, about 18 MB
render.yaml               Render blueprint: static site, publish path `site`
```

## Where it comes from

Generated from the full mirror in the sibling repo
[`tymuron/annaromeo-site`](https://github.com/tymuron/annaromeo-site) by
`tools/build_vastu.py` there. That script lifts `/main` to the root, gives the
five service pages clean URLs instead of Tilda's `<recid>-<uid>-<slug>` paths,
rewrites every internal link, copies only the assets these pages reference, and
writes `robots.txt` and `sitemap.xml` for this domain. To regenerate:

```
cd ../annaromeo-site
python3 tools/build_vastu.py ../annaromeo-vastu-site \
    --origin https://annaromeovastu.com --course ~/vastu-course-site
```

The course keeps its own repo. `--course` copies it in at `/course` and
rewrites only its metadata and the links that pointed at the expired
annaromeo.design. Its asset paths are all relative, so nothing collides with
this site's `/assets`. Re-run the command after the course changes.

Do not hand-edit `site/`; edit the mirror or the build script, then regenerate.

## Verified

Against the live Tilda page it replaces (`annaromeo.tilda.ws/main`): same
blocks in the same order, same height, identical visible text, no broken
images, no console errors, no failed requests. The only pixel difference is the
Vimeo background video caught on a different frame.

After every deploy, confirm the host actually serves every file. Render's
static upload has silently dropped files before while reporting success:

```
python3 ../annaromeo-site/tools/check_deploy.py https://annaromeovastu.com
```

## Still depends on Tilda

The forms and the "Записаться" cart on this site still post into the Tilda
project's lead inbox, exactly as they did before the move. They keep working
only while that Tilda subscription is alive, so they need their own endpoint,
such as a Telegram bot, before it lapses.

Two small JSON calls also still go to Tilda on page load and fail quietly if it
disappears: the phone mask asking `geo.tildaapi.one` for the visitor's country,
and the cart asking `store.tildaapi.one` for active discounts.
