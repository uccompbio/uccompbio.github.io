---
layout: page
title: Newsletters
permalink: /newsletters/
---

<p>Browse our newsletters below. Click <em>View</em> for an in-page preview, or <em>Download</em> for the PDF.</p>

<div class="row">
  {%- assign newsletter_pdfs = site.static_files | where_exp: "f", "f.path contains '/assets/newsletters/' and f.extname == '.pdf'" -%}
  {%- assign sorted_pdfs = newsletter_pdfs | sort: "name" | reverse -%}
  {%- for f in sorted_pdfs -%}
    {%- comment -%}
    Derive a human title from the filename:
    e.g., "COMBO_Newsletter_Summer2025.pdf" -> "COMBO Newsletter Summer 2025"
    {%- endcomment -%}
    {%- assign base = f.name | split: "." | first -%}
    {%- assign title = base | replace: "_", " " | replace: "-", " " -%}

    <div class="col-sm-6 col-lg-4" style="margin-bottom:1.25rem;">
      <div class="card h-100">
        {%- comment -%}
        If you’ve created thumbnails, name them the same as the PDF (jpg/png) and place in the same folder.
        The line below will try to use them; otherwise it skips the image.
        {%- endcomment -%}
        {%- assign jpg = f.path | replace: ".pdf", ".jpg" -%}
        {%- assign png = f.path | replace: ".pdf", ".png" -%}
        {%- assign thumb = nil -%}
        {%- for s in site.static_files -%}
          {%- if s.path == jpg or s.path == png -%}
            {%- assign thumb = s.path -%}
          {%- endif -%}
        {%- endfor -%}
        {%- if thumb -%}
          <img class="card-img-top" src="{{ thumb | relative_url }}" alt="Cover of {{ title }}">
        {%- endif -%}

        <div class="card-body d-flex flex-column">
          <h5 class="card-title" style="margin-bottom:0.5rem;">{{ title }}</h5>
          <p class="card-text">
            <small class="text-muted">{{ f.name }}</small>
          </p>
          <div class="mt-auto">
            <a class="btn btn-primary btn-sm" href="#" data-toggle="modal" data-target="#modal-{{ forloop.index }}">View</a>
            <a class="btn btn-outline-secondary btn-sm" href="{{ f.path | relative_url }}" download>Download</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal with embedded PDF -->
    <div class="modal fade" id="modal-{{ forloop.index }}" tabindex="-1" role="dialog" aria-labelledby="label-{{ forloop.index }}" aria-hidden="true">
      <div class="modal-dialog modal-xl" role="document" style="max-width:95%;">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="label-{{ forloop.index }}">{{ title }}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close" style="font-size:2rem; line-height:1;">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body" style="height:80vh;">
            <object data="{{ f.path | relative_url }}" type="application/pdf" width="100%" height="100%">
              <p>PDF preview not supported in this browser.
                <a href="{{ f.path | relative_url }}">Download the PDF</a> instead.</p>
            </object>
          </div>
          <div class="modal-footer">
            <a class="btn btn-outline-secondary" href="{{ f.path | relative_url }}" download>Download PDF</a>
            <button type="button" class="btn btn-primary" data-dismiss="modal">Close</button>
          </div>
        </div>
      </div>
    </div>

  {%- endfor -%}
</div>
