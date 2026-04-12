<%
const moduleId = templateParams.module_id;
const heading = templateParams.heading || "";
const emptyText = templateParams.empty_text || "";
const filtered = items.filter(item => {
  const mods = Array.isArray(item.modules) ? item.modules : [];
  return mods.includes(moduleId) && item.published !== false;
});
%>

<% if (heading) { %>
<h2><%- heading %></h2>
<% } %>

<% if (filtered.length === 0) { %>
  <% if (emptyText) { %>
<p><%- emptyText %></p>
  <% } %>
<% } else { %>
<ul class="related-items">
<% for (const item of filtered) { %>
  <li>
    <a href="<%- item.path %>"><%- item.title %></a>
    <% if (item.summary) { %>
      — <span><%- item.summary %></span>
    <% } %>
  </li>
<% } %>
</ul>
<% } %>