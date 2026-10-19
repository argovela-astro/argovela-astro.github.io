---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

<p style="font-size: 0.8em; margin-top: 0; margin-bottom: 1.5em;">
  <a href="https://orcid.org/0000-0002-5237-9433">ORCID</a>
  <span aria-hidden="true">&nbsp;·&nbsp;</span>
  <a href="https://ui.adsabs.harvard.edu/search/q=orcid%3A0000-0002-5237-9433">NASA ADS</a>
</p>

{% include base_path %}

{% assign selected_publications = site.publications | where: 'selected', true | sort: 'date' | reverse %}
{% for post in selected_publications %}
  {% include archive-single.html %}
{% endfor %}
