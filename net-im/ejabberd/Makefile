# $FreeBSD$

PORTNAME=	ejabberd
PORTVERSION=	18.01
CATEGORIES=	net-im
MASTER_SITES=	http://www.process-one.net/downloads/${PORTNAME}/${PORTVERSION}/:p1 \
		http://distfiles.pirateparty.in/ashish/:deps \
		LOCAL/ashish:deps
DISTFILES+=	${DISTNAME}.tgz:p1 ejabberd-deps-17.tar.xz:deps

MAINTAINER=	ashish@FreeBSD.org
COMMENT=	Free and Open Source distributed fault-tolerant Jabber server

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BUILD_DEPENDS=	erlc:lang/erlang \
		git:devel/git
RUN_DEPENDS=	erl:lang/erlang
LIB_DEPENDS=	libexpat.so:textproc/expat2 \
		libyaml.so:textproc/libyaml

USERS=		ejabberd
GROUPS=		${USERS}

GNU_CONFIGURE=	yes
USES=		cpe gmake pkgconfig shebangfix ssl
USE_RC_SUBR=	${PORTNAME}
NOPRECIOUSMAKEVARS=	yes
CPE_VENDOR=	process-one

OPTIONS_DEFINE=	ODBC PAM DOCS GRAPHICS MYSQL PGSQL ICONV RIAK ZLIB REDIS SIP SQLITE \
		STUN TOOLS FULLXML
OPTIONS_DEFAULT=	ICONV

EJABBERD_LIBDIR=lib/erlang/lib
MAKE_ENV=	PORTVERSION=${PORTVERSION}
CONFIGURE_ARGS+=--localstatedir=/var --libdir=${PREFIX}/${EJABBERD_LIBDIR}
MAKE_ARGS=	"CPPFLAGS=${CPPFLAGS}" "CFLAGS=${CFLAGS}" "LDFLAGS=${LDFLAGS}"
CPPFLAGS+=	-I${LOCALBASE}/include
CFLAGS+=	-I${LOCALBASE}/include
LDFLAGS+=	-L${LOCALBASE}/lib
PKGMESSAGE=	${WRKDIR}/pkg-message
PKGINSTALL=	${WRKDIR}/pkg-install
PLIST_SUB=	PORTNAME=${PORTNAME} PORTVERSION=${PORTVERSION} \
		EJABBERD_LIBDIR=${EJABBERD_LIBDIR}
SUB_FILES=	pkg-message pkg-install
SUB_LIST=	PORTNAME=${PORTNAME} PORTVERSION=${PORTVERSION} \
		USERS=${USERS} GROUPS=${GROUPS} SHAREMODE=${SHAREMODE} \
		SHAREOWN=${SHAREOWN} SHAREGRP=${SHAREGRP} \
		EJABBERD_LIBDIR=${EJABBERD_LIBDIR}
SHEBANG_FILES=	${WRKSRC}/ejabberdctl.template

OPTIONS_SUB=		yes
RIAK_DESC=	Riak database support
SIP_DESC=	SIP support
STUN_DESC=	STUN/TURN support
TOOLS_DESC=	Build development tools
FULLXML_DESC=	Full XML support
GRAPHICS_DESC=	Graphics images manipulation support
GRAPHICS_LIB_DEPENDS=	libgd.so:graphics/gd \
			libpng.so:graphics/png \
			libwebp.so:graphics/webp
GRAPHICS_USES=	jpeg
GRAPHICS_CONFIGURE_ENABLE=	graphics
PAM_CONFIGURE_ENABLE=	pam
ODBC_CONFIGURE_ENABLE=	odbc
MYSQL_CONFIGURE_ENABLE=	mysql
PGSQL_CONFIGURE_ENABLE=	pgsql
RIAK_CONFIGURE_ENABLE=	riak
ICONV_CONFIGURE_ENABLE=	iconv
REDIS_CONFIGURE_ENABLE=	redis
SIP_CONFIGURE_ENABLE=	sip
STUN_CONFIGURE_ENABLE=	stun
SQLITE_CONFIGURE_ENABLE=sqlite
TOOLS_CONFIGURE_ENABLE=	tools
FULLXML_CONFIGURE_ENABLE=	full-xml
ZLIB_CONFIGURE_ENABLE=	zlib
SQLITE_USES=		sqlite
ICONV_USES=		iconv:wchar_t
ICONV_LDFLAGS=		${ICONV_LIB}
DOCS_MAKE_ARGS_OFF=	NOPORTDOCS=yes

post-extract:
	@${LN} -s ${WRKDIR}/deps ${WRKSRC}/

post-patch:
	@${REINPLACE_CMD} -e 's,^convert,${LOCALBASE}/bin/convert,' ${WRKSRC}/tools/captcha.sh

pre-configure:
	@${REINPLACE_CMD} -e 's,/usr/local,${LOCALBASE},g' ${WRKSRC}/configure
	@${REINPLACE_CMD} -e 's,%%LOCALBASE%%,${LOCALBASE},g' \
		${WRKSRC}/ejabberdctl.template

pre-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}

post-install-GRAPHICS-on:
	@${CHMOD} +x ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/eimp-1.0.2/priv/bin/eimp
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/eimp-1.0.2/priv/bin/eimp

post-install-PAM-on:
	${INSTALL} -m 444 ${FILESDIR}/pam_ejabberd \
		${STAGEDIR}${PREFIX}/etc/pam.d/ejabberd
	${INSTALL} -m 0750 ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/epam-1.0.3/priv/bin/epam \
		${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/priv/bin/epam
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/epam-1.0.3/priv/bin/epam \
		${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/priv/bin/epam

post-install:
	@${FIND} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR} -type f \
		-name '*.so' -exec ${STRIP_CMD} {} \;
	@${MKDIR} ${STAGEDIR}${DATADIR}
	${INSTALL_SCRIPT} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/priv/bin/captcha.sh \
		${STAGEDIR}${DATADIR}
	@${REINPLACE_CMD} -r -e '/captcha_cmd:/s,"[^"]+","${DATADIR}/captcha.sh",' ${STAGEDIR}${ETCDIR}/${PORTNAME}.yml.example
	@${CHMOD} +x ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/fs-3.4.0/priv/kqueue
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/fs-3.4.0/priv/kqueue

.include <bsd.port.mk>
