# $FreeBSD$

PORTNAME=	faktory
DISTVERSION=	0.7.0-1
DISTVERSIONPREFIX=	v

CATEGORIES=	databases

MAINTAINER=	wes@wezm.net
COMMENT=	Background job repository and UI

LICENSE=	GPLv3
LICENSE_FILE=	${WRKSRC}/LICENSE

USES=		compiler go

USE_GITHUB=	yes
GH_ACCOUNT=	contribsys
GH_SUBDIR:=	src/github.com/${GH_ACCOUNT}/${PORTNAME}

GH_TUPLE=	sirupsen:logrus:v1.0.0:logrus/src/github.com/sirupsen/logrus \
		contribsys:gorocksdb:844b933:gorocksdb/src/github.com/contribsys/gorocksdb \
		stretchr:testify:v1.1.4:testify/src/github.com/stretchr/testify \
		apex:log:v1.0.0:log/src/github.com/apex/log \
		chzyer:readline:v1.4:readline/src/github.com/chzyer/readline \
		davecgh:go-spew:v1.1.0:spew/src/github.com/davecgh/go-spew \
		justinas:nosurf:7182011:nosurf/src/github.com/justinas/nosurf \
		pkg:errors:v0.8.0:errors/src/github.com/pkg/errors \
		pmezard:go-difflib:v1.0.0:difflib/src/github.com/pmezard/go-difflib \

LIB_DEPENDS=	librocksdb.so:databases/rocksdb
BUILD_DEPENDS=	ego:devel/go-ego \
		go-bindata:devel/go-bindata

PLIST_FILES=	bin/faktory bin/faktory-cli

GOENV=		GOPATH=${WRKSRC} CGO_CFLAGS="${CGO_CFLAGS}" \
		CGO_LDFLAGS="${CGO_LDFLAGS}" GOBIN=""

do-build:
	@(cd ${WRKSRC}/src/github.com/${GH_ACCOUNT_DEFAULT}/${PORTNAME} && \
		${SETENV} ${MAKE_ENV} ${GOENV} ${GO_CMD} generate github.com/contribsys/faktory/webui)
	@(cd ${WRKSRC}/src/github.com/${GH_ACCOUNT_DEFAULT}/${PORTNAME} && \
		${SETENV} ${MAKE_ENV} ${GOENV} ${GO_CMD} build -ldflags="-s -w" -p ${MAKE_JOBS_NUMBER} -o ${PORTNAME}-cli cmd/faktory-cli/repl.go)
	@(cd ${WRKSRC}/src/github.com/${GH_ACCOUNT_DEFAULT}/${PORTNAME} && \
		${SETENV} ${MAKE_ENV} ${GOENV} ${GO_CMD} build -ldflags="-s -w" -p ${MAKE_JOBS_NUMBER} -o ${PORTNAME} cmd/faktory/daemon.go)

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/${PORTNAME}-cli ${STAGEDIR}${PREFIX}/bin/${PORTNAME}-cli
	${INSTALL_PROGRAM} ${WRKSRC}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin/${PORTNAME}

.include <bsd.port.mk>
