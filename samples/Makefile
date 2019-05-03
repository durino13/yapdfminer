CMP=:

PDF2TXT=PYTHONPATH=.. ../tools/pdf2txt.py -p1 -V

FREE= \
	simple1 \
	simple2
	# Removed: require CJK support:
	# simple3
	# jo

NONFREE= \
	nonfree/dmca \
	nonfree/f1040nr \
	nonfree/i1040nr \
	nonfree/naacl06-shinyama \
	nonfree/one-byte-identity
	# Removed: require CJK support:
	# nonfree/kampo
	# nonfree/nlp2004slides

TESTS=$(FREE) $(NONFREE)

CRYPT_PASS1=foo
CRYPT_PASS2=baz
CRYPT_BASE=encryption/base
CRYPTS= \
	encryption/rc4-40 \
	encryption/rc4-128 \
	encryption/aes-128 \
	encryption/aes-128-m \
	encryption/aes-256 \
	encryption/aes-256-m

all: privatepdfs tests crypts

privatepdfs:
	@[ -d ../tmp/private ] || mkdir ../tmp/private
	@for i in private/*.pdf; do \
		echo $$i; \
		$(PDF2TXT) -t text -o ../tmp/$$i.txt $$i || exit 1; \
	done

tests:
	@[ -d ../tmp ] || mkdir ../tmp
	@[ -d ../tmp/nonfree ] || mkdir ../tmp/nonfree
	@for i in $(TESTS); do \
		echo $$i; \
		$(PDF2TXT) -t html -o ../tmp/$$i.html $$i.pdf || exit 1; \
		$(CMP) $$i.html.ref ../tmp/$$i.html || exit 1; \
		$(PDF2TXT) -t xml -o ../tmp/$$i.xml $$i.pdf || exit 1; \
		$(CMP) $$i.xml.ref ../tmp/$$i.xml || exit 1; \
		$(PDF2TXT) -t text -o ../tmp/$$i.txt $$i.pdf || exit 1; \
		$(CMP) $$i.txt.ref ../tmp/$$i.txt || exit 1; \
	done

crypts:
	@[ -d ../tmp/encryption ] || mkdir ../tmp/encryption
	@for i in $(CRYPTS); do \
		echo $$i; \
		$(PDF2TXT) -t xml -P $(CRYPT_PASS1) -o ../tmp/$$i.1.xml $$i.pdf || exit 1; \
		$(CMP) $(CRYPT_BASE).xml ../tmp/$$i.1.xml || exit 1; \
		$(PDF2TXT) -t xml -P $(CRYPT_PASS2) -o ../tmp/$$i.2.xml $$i.pdf || exit 1; \
		$(CMP) ../tmp/$$i.2.xml $(CRYPT_BASE).xml || exit 1; \
	done

test:
	make all CMP='git diff --no-index --no-prefix'
