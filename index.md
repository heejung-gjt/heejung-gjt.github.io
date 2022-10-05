---
title: developer to study
---

<p>“Most good programmers do programming not because they expect to get paid or get adulation by the public, but because it is fun to program.”
</p>

<p class="editor-link"><a href="cloudcannon:collections/" class="btn"><strong>&#9998;</strong> Update Change Log</a></p>

<div class="changelog">
	{% for change in site.docs %}
		<a href="{{ change.url }}" style="text-decoration: none"><div class="changelog-item">
			<h3>{{ change.title }}</h3>
			<p><span class="date">{{ change.date | date: "%B %d, %Y" }}</span> <span class="badge {{ change.type }}">{{ change.type }}</span></p>

            <!-- {{ change.title }} -->

			<!-- {{ change.title }}

			<p class="editor-link"><a href="cloudcannon:collections/{{ change.path }}" class="btn"><strong>&#9998;</strong> Update Entry</a></p> -->
		</div></a>
	{% endfor %}
</div>