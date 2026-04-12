```{=html}
<%
function asArray(v) {
  if (Array.isArray(v)) return v;
  if (v === undefined || v === null || v === "") return [];
  return [v];
}

function isPublished(v) {
  return !(v === false || v === "false");
}

function slugifyTag(s) {
  return String(s)
    .toLowerCase()
    .trim()
    .replace(/[^a-z0-9]+/g, "-")
    .replace(/^-+|-+$/g, "");
}

const heading = templateParams.heading || "";

/* alles erst mal nur nach published filtern */
const publishedItems = items.filter(item => isPublished(item.published));

/* Module NICHT über path, sondern über id erkennen */
const moduleItems = publishedItems.filter(item =>
  String(item.id || "").startsWith("module-")
);

/* Rest sind hier die eigentlichen Materials */
const materialItems = publishedItems.filter(item =>
  !String(item.id || "").startsWith("module-")
);

/* sortieren */
moduleItems.sort((a, b) => Number(a.order || 999999) - Number(b.order || 999999));
materialItems.sort((a, b) => Number(a.order || 999999) - Number(b.order || 999999));
%>

<% if (heading) { %>
<h1 class="related-section-title"><%= heading %></h1>
<% } %>

<% for (const mod of moduleItems) { %>
  <%
  const grouped = materialItems.filter(item => {
    const mods = asArray(item.modules);
    return mods.includes(mod.id);
  });
  %>

  <% if (grouped.length > 0) { %>
  <section class="module-group">
    <h2 class="module-group-title"><%= mod.title %></h2>

    <div class="related-cards">
    <% for (const item of grouped) { %>
      <article class="related-card">
        <div class="related-card-body">
          <h3 class="related-card-title">
            <a href="<%= item.path %>"><%= item.title %></a>
          </h3>

          <% if (item.summary) { %>
          <p class="related-card-summary"><%= item.summary %></p>
          <% } %>
        </div>

        <% const tags = asArray(item.labels); %>
        <% if (tags.length > 0) { %>
        <div class="related-card-tags">
          <% for (const tag of tags) { %>
          <span class="related-card-tag tag-<%= slugifyTag(tag) %>"><%= tag %></span>
          <% } %>
        </div>
        <% } %>
      </article>
    <% } %>
    </div>
  </section>
  <% } %>
<% } %>
```