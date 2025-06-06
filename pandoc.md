# Controlling the output PDF

For example, if you want to use `Arial` font and a 1-inch margin, run this:

`pandoc input.md -o output.pdf --pdf-engine=xelatex -V geometry:margin=1in -V mainfont="Arial"`
