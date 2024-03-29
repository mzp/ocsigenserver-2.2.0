include ../../Makefile.config

LIBS     := -package netstring,lwt.ssl,tyxml -I ../baselib
OCAMLC   := $(OCAMLFIND) ocamlc${BYTEDBG}
OCAMLOPT := $(OCAMLFIND) ocamlopt ${OPTDBG}
OCAMLDOC := $(OCAMLFIND) ocamldoc
OCAMLDEP := $(OCAMLFIND) ocamldep

all: byte opt

### Common files ###

FILES := multipart.ml            \
	 http_headers.ml         \
	 ocsigen_cookies.ml      \
         ocsigen_http_frame.ml   \
	 ocsigen_headers.ml      \
	 http_lexer.ml	         \
         framepp.ml              \
	 ocsigen_http_com.ml     \
	 ocsigen_charset_mime.ml \
         ocsigen_senders.ml      \

PREDEP := http_lexer.ml   \

byte: http.cma
opt: http.cmxa

http.cma: $(FILES:.ml=.cmo)
	${OCAMLC} -a -o $@ $^
http.cmxa: $(FILES:.ml=.cmx)
	${OCAMLOPT} -a -o $@ $^

##########

%.ml: %.mll
	$(OCAMLLEX) $<
%.cmi: %.mli
	$(OCAMLC) ${LIBS} -c $<
%.cmo: %.ml
	$(OCAMLC) ${LIBS} -c $<
%.cmx: %.ml
	$(OCAMLOPT) ${LIBS} -c $<
%.cmxs: %.cmxa
	$(OCAMLOPT) -shared -linkall -o $@ $<

## Clean up

clean:
	-rm -f *.cm[ioax] *.cmxa *.cmxs *.o *.a *.annot
	-rm -f ${PREDEP}
distclean: clean
	-rm -f *~ \#* .\#*

## Dependencies

depend: ${PREDEP}
	$(OCAMLDEP) ${LIBS} *.mli *.ml > .depend

FORCE:
-include .depend
