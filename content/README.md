## New content folder (same as /docs)  guide

Content from : https://github.com/atwriter/new_doks_site/pull/1/files

  ### What I want

- New content folder as the same level as /docs 
  - like abc.com/docs , abc.com/newfolder
- The same css style as /docs 

### How to

This case, `docs2` as new folder

1. update `assets/scss/components/_alerts.scss `

```
  margin-right: 0.75rem;
}

.docs .alert {
.docs .alert,
.docs2 .alert {
  margin: 2rem -1.5rem;
}
```

2. Add those files

```
./layouts/docs2/list.html
./layouts/docs2/single.html
./layouts/partials/sidebar/docs2-menu.html
```

list.html

```
{{ define "main" }}
<div class="row justify-content-center">
  <div class="col-md-12 col-lg-10 col-xl-8">
    <article>
      <h1 class="text-center">{{ .Title }}</h1>
      <div class="text-center">{{ .Content }}</div>
			<div class="card-list">
				{{ $currentSection := .CurrentSection }}
				{{ range where .Site.RegularPages.ByTitle "Section" .Section }}
					{{ if in (.Permalink | string) $currentSection.RelPermalink }}
						<div class="card my-3">
							<div class="card-body">
								<a class="stretched-link" href="{{ .Permalink }}">{{ .Params.title | title }} &rarr;</a>
							</div>
						</div>
					{{ end }}
				{{ end }}
			</div>
    </article>
  </div>
</div>
{{ end }}
```

single.html

```
{{ define "main" }}
	<div class="row flex-xl-nowrap">
		<div class="col-lg-5 col-xl-4 docs-sidebar">
			<nav class="docs-links" aria-label="Main navigation">
				{{ partial "sidebar/docs2-menu.html" . }}
			</nav>
		</div>
		{{ if ne .Params.toc false -}}
		<nav class="docs-toc d-none d-xl-block col-xl-3" aria-label="Secondary navigation">
			{{ partial "sidebar/docs-toc.html" . }}
		</nav>
		{{ end -}}
		{{ if .Params.toc -}}
		<main class="docs-content col-lg-11 col-xl-9">
		{{ else -}}
		<main class="docs-content col-lg-11 col-xl-9 mx-xl-auto">
		{{ end -}}
			{{ if .Site.Params.options.breadCrumb -}}
				<!-- https://discourse.gohugo.io/t/breadcrumb-navigation-for-highly-nested-content/27359/6 -->
				<nav aria-label="breadcrumb">
					<ol class="breadcrumb">
						{{ partial "main/breadcrumb" . -}}
						<li class="breadcrumb-item active" aria-current="page">{{ .Title }}</li>
					</ol>
				</nav>
			{{ end }}
			<h1>{{ .Title }}</h1>
			<p class="lead">{{ .Params.lead | safeHTML }}</p>
			{{ partial "main/headline-hash.html" .Content }}
			{{ if .Site.Params.editPage -}}
				{{ partial "main/edit-page.html" . }}
			{{ end -}}
			{{ partial "main/docs-navigation.html" . }}
		</main>
	</div>
{{ end }} 

```

docs2-menu.html 

```
{{ $currentPage := . -}}
{{ range .Site.Menus.docs2 -}}
  <h3>{{ .Name }}</h3>
  {{ if .HasChildren -}}
  <ul class="list-unstyled">
    {{ range .Children -}}
      {{- $active := or ($currentPage.IsMenuCurrent "docs2" .) ($currentPage.HasMenuCurrent "docs2" .) -}}
      {{- $active = or $active (eq $currentPage.Section .Identifier) -}}
      <li><a class="docs-link{{ if $active }} active{{ end }}" href="{{ .URL | absURL }}">{{ .Name }}</a></li>
    {{ end -}}
  </ul>
  {{ end -}}
{{ end -}} 
```

