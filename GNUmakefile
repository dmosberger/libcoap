TOP=..
include $(TOP)/Make.common

CPPFLAGS += -D_GNU_SOURCE -I. -Iinclude -Iinclude/coap

LIBS = libcoap.a

OFILES = address async block coap_event coap_io coap_notls \
	coap_session coap_time debug encode mem net option \
	pdu resource str subscribe uri
LIBOBJS = $(foreach obj,$(OFILES),$O/coap-$(obj).o)

ifneq ($(PLATFORM),egauge2)
PROGS	= coap-client
endif

$O/coap-%.o: %.c
	$(CC) -c $(CPPFLAGS) $(CFLAGS) $< -o $@

TARGETS	= $(foreach prog,$(LIBS) $(PROGS),$O/$(prog))

VPATH += src examples

all:	 $(TARGETS)

$O/libcoap.a: $O/libcoap.a($(LIBOBJS))
$O/coap-client: $O/libcoap.a $O/coap-client.o $O/coap_list.o

$O/coap-client.o: client.c
	$(CC) -c $(CPPFLAGS) $(CFLAGS) $< -o $@

clean:
	rm -f $O/*.o $(TARGETS)

dist:
	mkdir -p $(DISTDIR)/usr/lib/egauge/
	(for p in $(PROGS); do \
		$(INSTALLBIN) $O/$$p $(DISTDIR)/usr/lib/egauge/$$p.new; \
	done)

release:

.PHONY: clean
