# Examples

Before/after rewrites. Most of them document the same feature: a store whose product images are copied from Shopify to its inventory system, with a card on the variant page that shows what happened to each image. The "before" versions are mostly accurate. They fail because they're written for the wrong reader, or in the wrong shape. They're in English so the shape is easy to copy, and the real text goes in the users' language. The exception is example 5, which stays in Spanish because the failure it shows is Spanish.

1. The developer's manual → a page for the person at the store
2. The brochure → what the user can now do
3. A wall of text → steps
4. An error, explained → what to do about it
5. Spanish hybrids → the words on the screen

---

## 1. The developer's manual → a page for the person at the store

*"Document the image sync result card for the store staff."*

**Before:**
> ## Image Sync Results
>
> When a product is updated in Shopify, the `SyncImages` job evaluates each variant and returns an `ImageSyncResultCode`. Results are persisted to the `image_sync_results` table and exposed via `GET /api/admin/inventory/variants/{id}/image-sync-result`. The admin block extension renders the latest result on the variant page. Possible codes: `UPDATED`, `SKIPPED_SAME`, `SKIPPED_LOCKED`, `NOT_FOUND`, `ERROR`.

What gives it away:
- a job, an enum, a table and an endpoint, none of which the staff will ever see
- the codes are the system's names, not what the card says
- no task: the reader can't tell what to do with this

**After:**
> ## See why a variant's image didn't update
>
> When you change a product's image in Shopify, it's copied to the inventory system. If an image there looks wrong, the variant page tells you what happened.
>
> 1. In Shopify, open the product.
> 2. In **Variants**, click the variant.
> 3. Scroll to the **Image sync** card.
>
> The card shows the result of the last sync:
>
> | The card says | What it means | What to do |
> |---|---|---|
> | **Updated** | The new image was copied. | Nothing. It can take up to 15 minutes to show in the inventory system. |
> | **Same image** | The image hadn't changed, so nothing was copied. | Nothing. |
> | **Locked by hand** | Someone set this image directly in the inventory system, so Shopify doesn't overwrite it. | If you want Shopify's image, ask the inventory team to remove the manual image. |
> | **Not found** | This variant doesn't exist in the inventory system. | Check the variant's barcode, or ask the inventory team to create it. |
>
> [Screenshot: the variant page, with the Image sync card highlighted]

What changed: the title is the task, the internal codes became the card's own labels, and each result says what to do. The table earns its place, because the reader looks up one row.

---

## 2. The brochure → what the user can now do

*"Write the 'what's new' note for the image sync card."*

**Before:**
> 🚀 We're thrilled to introduce Image Sync Insights, a powerful new feature that brings unprecedented visibility into your catalog! Seamlessly track every image across your systems and never wonder again. It's a game-changer for your team!

What gives it away:
- "thrilled", "powerful", "seamlessly", "game-changer", and not one concrete fact
- the reader still doesn't know where it is or what it shows
- a name the screen doesn't use ("Image Sync Insights")

**After:**
> **New: see why an image didn't update**
>
> Each variant page now has an **Image sync** card. It shows whether the last image was copied to the inventory system, and if it wasn't, why. Until now you had to ask a developer to check.
>
> You'll find it at the bottom of the variant page in Shopify.

What changed: what you can do, where it is, and what it replaces. Three sentences carry more than the paragraph of adjectives.

---

## 3. A wall of text → steps

*"How do I replace the image of a variant?"*

**Before:**
> To replace the image you first need to make sure you have permission to edit products, then go to Products in the menu and look for the product, and once you've opened it you'll see the variants, where you can click the one you want and, in the image area, use the replace option to upload the new file, which must be a JPG or PNG under 5 MB, and then just save and it will be updated.

What gives it away:
- six actions and two requirements in one sentence; the reader loses their place each time they look at the screen
- "just save", and "it will be updated" without saying when or where
- the file requirements arrive after the reader already picked a file

**After:**
> ## Replace a variant's image
>
> **Before you start:** you need permission to edit products, and the image must be a JPG or PNG under 5 MB.
>
> 1. In the side menu, click **Products**.
> 2. Open the product.
> 3. In **Variants**, click the variant you want to change.
> 4. Under **Image**, click **Replace** and choose the file.
> 5. Click **Save**.
>
> The variant page shows the new image right away. The store and the inventory system show it within 15 minutes.

What changed: the requirements moved before the steps, each step has one action with the place first, and the result says when and where.

---

## 4. An error, explained → what to do about it

*"Write the help entry for the upload errors."*

**Before:**
> Upload errors may occur for a variety of reasons, including file size, format or connectivity issues. If you experience an error, please try again or contact support.

What gives it away:
- "a variety of reasons", without saying which one the reader has
- no message quoted, so the reader can't match it to what they see
- "try again" for an error that will fail again

**After:**
> ## If the image doesn't upload
>
> **"The file is too large"**: the image is over 5 MB. Make it smaller, or save it as JPG, and try again.
>
> **"Format not supported"**: only JPG and PNG are accepted. Save the image in one of those formats and try again.
>
> **The upload stays at 0%**: usually the connection dropped. Reload the page and try again. If it keeps happening, write to the support channel with the product name.

What changed: each message is quoted as the reader sees it, with its cause and one action. The last case says who to contact and what to send them.

---

## 5. Spanish hybrids → the words on the screen

This one stays in Spanish because the failure is Spanish.

**Antes:**
> Para cambiar la imagen, logueate en el dashboard, clickeá en Products y seteá la nueva imagen en la variante. Después uploadeá el archivo y el sistema se encarga de sincronizarlo automáticamente.

What gives it away:
- "logueate", "clickeá", "seteá", "uploadeá", hybrid verbs nobody writes in a manual
- "dashboard" and "Products", when the screen says "Panel" and "Productos"
- "el sistema se encarga de", which hides when the change shows up

**Después:**
> 1. En el **Panel**, hacé clic en **Productos**.
> 2. Abrí el producto.
> 3. En **Variantes**, hacé clic en la variante.
> 4. En **Imagen**, hacé clic en **Reemplazar** y elegí el archivo nuevo.
> 5. Hacé clic en **Guardar**.
>
> La tienda muestra la imagen nueva en 15 minutos como máximo.

The labels are the screen's own, the verbs are plain Spanish, and "el sistema se encarga" became the number the reader cares about. It uses "vos" because the screens of this product do; if they used "usted", so would the guide.
