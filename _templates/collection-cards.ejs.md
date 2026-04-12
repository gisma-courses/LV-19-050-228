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
const filtered = items.filter(item => isPublished(item.published));
%>

<% if (heading) { %>
<h2 class="related-section-title"><%= heading %></h2>
<% } %>

<div class="related-cards">
<% for (const item of filtered) { %>
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
```