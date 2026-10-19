---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

[ORCID](https://orcid.org/0000-0002-5237-9433) · [NASA ADS](https://ui.adsabs.harvard.edu/search/q=orcid%3A0000-0002-5237-9433) · [Download publication list (PDF)]({{ '/files/Publication_List.pdf' | relative_url }})

{% assign publications = site.publications | sort: 'date' | reverse %}
{% assign first_author_papers = publications | where: 'authorship', 'first' %}
{% assign coauthor_papers = publications | where: 'authorship', 'coauthor' %}

## First author and joint first author

{% for post in first_author_papers %}
  {% include publication-entry.html number=forloop.index %}
{% endfor %}

## Coauthor

{% for post in coauthor_papers %}
  {% assign publication_number = forloop.index | plus: first_author_papers.size %}
  {% include publication-entry.html number=publication_number %}
{% endfor %}
