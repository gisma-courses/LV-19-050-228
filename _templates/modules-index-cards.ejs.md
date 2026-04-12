```{=html}
<%
const filtered = items.filter(item => item.published !== false);
%>

<div class="related-cards">
<% for (const item of filtered) { %>
  <article class="related-card">
    <% if (item["title-block-banner"]) { %>
    <div class="related-card-image">
      <img src="<%= item['title-block-banner'] %>" alt="<%= item.title %>">
    </div>
    <% } %>

    <div class="related-card-body">
      <h3 class="related-card-title">
        <a href="<%= item.path %>"><%= item.title %></a>
      </h3>

      <% if (item.summary) { %>
      <p class="related-card-summary"><%= item.summary %></p>
      <% } %>
    </div>
  </article>
<% } %>
</div>
```