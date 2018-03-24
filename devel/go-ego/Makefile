# $FreeBSD$

PORTNAME=	ego
DISTVERSIONPREFIX=	v
DISTVERSION=	0.2.0-6
DISTVERSIONSUFFIX=	-g64fcc5d
CATEGORIES=	devel

MAINTAINER=	wes@wezm.net
COMMENT=	An ERB-style templating language for Go

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

USE_GITHUB=	yes
GH_ACCOUNT=	benbjohnson

USES=		go
GO_PKGNAME=	github.com/${GH_ACCOUNT}/${PORTNAME}
GO_TARGET=	github.com/${GH_ACCOUNT}/${PORTNAME}/cmd/ego

PLIST_FILES=	bin/ego

.include <bsd.port.mk>
