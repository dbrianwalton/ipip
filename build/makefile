DOCroot = ..
XML = $(DOCroot)/src
XSL = $(DOCroot)/xsl
HTML = $(DOCroot)/html

MB = ~/mathbook
RNG = ~/jing-trang

HTML_XSL = waltbook-html.xsl
LATEX_XSL = waltbook-latex.xsl

XML_base = ipip.ptx
LATEX_base = ipip
LATEX_single = test-section

.PHONY: rng wladq.validate

rng: wladq.validate ;

wladq.validate:
	java -classpath $(RNG)/build -Dorg.apache.xerces.xni.parser.XMLParserConfiguration=org.apache.xerces.parsers.XIncludeParserConfiguration -jar $(RNG)/build/jing.jar $(MB)/Schema/pretext.rng $(XML)/wladq.xml > wladq.validate

.PHONY: html_files html_images pdf single

html_single: $(HTML)
	cp -a ${XML}/js ${HTML}/
	cd $(HTML) && xsltproc --xinclude $(XSL)/$(HTML_XSL) $(XML)/$(XML_single)

html_single_images: $(HTML)/images
	cp -a $(DOCroot)/img/ $(HTML)/images/
	$(MB)/script/mbx --include $(DOCroot)/src/data/ -c latex-image -d $(HTML)/images/ -f svg $(XML)/$(XML_single)


html_files: $(HTML) $(HTML)/images
	#cp -a ${XML}/js ${HTML}/
	cd $(HTML) && xsltproc --xinclude $(XSL)/$(HTML_XSL) $(XML)/$(XML_base)
	if [ -d "$(XML)/code" ] ; then if [ ! -d "$(HTML)/code" ] ; then mkdir $(HTML)/code ; fi ; cp -R $(XML)/code/ $(HTML)/code/ ; fi
	if [ -d "$(XML)/js" ] ; then if [ ! -d "$(HTML)/js" ] ; then mkdir $(HTML)/js ; fi ; cp -R $(XML)/js/ $(HTML)/js/ ; fi

$(HTML):
	mkdir $(HTML)

$(HTML)/images: $(HTML)
	if [ -d "$(DOCroot)/img" ] ; then if [ ! -d "$(HTML)/images" ] ; then mkdir $(HTML)/images ; fi ; cp -a $(DOCroot)/img/ $(HTML)/images/ ; fi

html_images: $(HTML)/images
	cp -a $(DOCroot)/img/ $(HTML)/images/
	$(MB)/script/mbx --include $(DOCroot)/src/data/ -c latex-image -d $(HTML)/images/ -f svg $(XML)/$(XML_base)

single: $(LATEX_single).pdf;

.PHONY: $(LATEX_single).tex

$(LATEX_single).tex:
	xsltproc --xinclude $(XSL)/$(LATEX_XSL) $(XML)/$(XML_single)

$(LATEX_single).pdf: images $(LATEX_single).tex
	cp -a $(DOCroot)/img/ images/
	cp -a $(DOCroot)/src/data/ data/
	pdflatex $(LATEX_single).tex

pdf: $(LATEX_base).pdf ;

latex: $(LATEX_base).tex ;

images:
	mkdir images
data:
	mkdir data

.PHONY: $(LATEX_base).tex

$(LATEX_base).tex:
	xsltproc --xinclude $(XSL)/$(LATEX_XSL) $(XML)/$(XML_base)

$(LATEX_base).pdf: images $(LATEX_base).tex
	cp -a $(DOCroot)/img/ images/
	#cp -a $(DOCroot)/src/data/ data/
	pdflatex $(LATEX_base).tex
	pdflatex $(LATEX_base).tex
