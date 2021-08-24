---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Air Sea Circulation on Titan"
subtitle: "Thoughts on the recently published Rafkin & Soto (2020)."
summary: ""
authors: []
tags: []
categories: []
date: 2021-02-16T22:52:02-07:00
lastmod: 2021-02-16T22:52:02-07:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  placement: 3
  caption: "Cassini radar image of lakes on Titan, taken in 2006. Bolsena Lacus is at lower right."
  focal_point: "Smart"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: [hydrology]
---

Imaging and radar observations from the Cassini mission showed that the polar regions of Titan are filled with a number of lakes and seas, particularly the north pole. Cassini also observed clouds over these lakes and possible fogs near the surface of these lakes. From these observations, some scientists suggested that the interaction between the atmosphere and the lakes may be creating the observed fogs and clouds. However, only a few studies had looked at how heat, methane, and momentum are exchanged between the lakes and the atmosphere on Titan. For this reason, Scot Rafkin and I used a Titan mesoscale atmospheric model to investigate the exchange of heat, methane, and momentum between the lakes and atmosphere, a process often called [air-sea interactions](https://www.britannica.com/science/air-sea-interface).

Our work on Titan air-sea interactions was published last year as [Rafkin and Soto (2020)](/publication/rafkin-2020/). For this work, I modified an "off-the-shelf" version of the [Weather Research & Forecasting (WRF)[^1]](https://www.mmm.ucar.edu/weather-research-and-forecasting-model) to work for Titan, which I called the mesoscale Titan WRF (mtWRF)[^2]. We set up the model as an idealized, [mesoscale](https://glossary.ametsoc.org/wiki/Mesoscale) atmospheric simulation. The 







[^1]: The acronym WRF is often pronounced as "wharf" (or Worf, for the Star Trek fans.)
[^2]: I pronounce this as "met-wharf". Next time, I should do a better job in creating the acronym.

