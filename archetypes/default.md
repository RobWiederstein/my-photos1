---
#
# "$" custom/local variable "imgglob"
# :=
# {{ $address := "123 Main St." }}
# Join path elements into a single path. "path.Join"
# {{ path.Join "partial" "news.html" }} → "partial/news.html"
# printf Formats a string using the standard fmt.Sprintf function.
# "*%s" the uninterpreted bytes of the string or slice
# printf FORMAT INPUT {{ i18n ( printf "combined_%s" $var ) }}
{{- $imgglob := printf "*%s" (path.Join .File.Dir "**") -}}
{{- $resources := where (resources.Match $imgglob) "ResourceType" "image" }}
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
# Returns the length of a variable according to its type.
albumthumb: "{{ cond (gt (len $resources) 0) (index $resources 0) "" }}"
draft: false
## Optional additional meta info for resources list
#  alt: Image alternative and screen-reader text
#  phototitle: A title for the photo
#  description: A sub-title or description for the photo
resources:
{{ range $elem_index, $elem_val := $resources -}}
- src: "{{ . }}"
{{ end -}}
---
