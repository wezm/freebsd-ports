# Created by: Cosmin Stroe <cstroe1@uic.edu>
# $FreeBSD$

PORTNAME=	zidrav
PORTVERSION=	1.2.0
PORTREVISION=	1
CATEGORIES=	sysutils
MASTER_SITES=	SF/${PORTNAME}/${PORTNAME}4unix/${PORTVERSION}
DISTNAME=	zidrav4unix-${PORTVERSION}

MAINTAINER=	dinoex@FreeBSD.org
COMMENT=	File corruption detection and repair program

LICENSE=	GPLv2

BROKEN_FreeBSD_12=	Regression C++

ALL_TARGET=	zidrav
PLIST_FILES=	bin/zidrav man/man1/zidrav.1.gz

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/zidrav ${STAGEDIR}${PREFIX}/bin/
	${INSTALL_MAN} ${WRKSRC}/zidrav.1 ${STAGEDIR}${PREFIX}/man/man1/

.include <bsd.port.options.mk>

.if ${ARCH} == "i386"
CFLAGS+=	-Dcrc32_type=long
.else
CFLAGS+=	-Dcrc32_type=int
.endif

.if ! empty(ARCH:Mmips*) || ! empty(ARCH:Mpowerpc*) || ${ARCH} == "sparc64"
CFLAGS+=	-DCPU_BIGENDIAN
.endif

.include <bsd.port.mk>
