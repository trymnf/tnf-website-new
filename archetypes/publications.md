+++
title = "{{ replace .Name "-" " " | title }}"
date = "{{ .Date }}"

draft= false
fave= true
file= FILE
doi= DOI
data=DATA
author= "Trym Nohr Fjørtoft"
status= "Published"
type= "published"

citation= '“TITLE” <em>Journal</em> (forthcoming).'
tags= ["publications"]
publishdate= {{ .Date }}
abstract= true

# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."
+++

This is a page about »{{ replace .Name "-" " " | title }}«.

