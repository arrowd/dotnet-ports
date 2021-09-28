PORTNAME=	dotnet6-installer
DISTVERSION=	6.0.100-rc.1.21458.32
DISTVERSIONPREFIX=	v
CATEGORIES=	lang devel
MASTER_SITES=	https://github.com/Thefrank/dotnet-freebsd-crossbuild/releases/download/v6.0.0-rc.1.21451.13/:bootstrap
DISTFILES=	bootstrap-dotnet-sdk-6.0.100-rc.1.21458.32-freebsd-x64.tar.gz:bootstrap

MAINTAINER=	your@email.here
COMMENT=	Auxiliary port for producing .NET bootstrap package

LICENSE=	MIT

NO_PACKAGE=	This port is not intended to produce a usual FreeBSD package. See Makefile for instructions on how to use it.

BUILD_DEPENDS=	bash:shells/bash \
		curl:ftp/curl
LIB_DEPENDS=	libunwind.so:devel/libunwind \
		libicuuc.so:devel/icu \
		libinotify.so:devel/libinotify

USES=		shebangfix

SHEBANG_GLOB=	*.sh

USE_GITHUB=	yes
GH_ACCOUNT=	dotnet
GH_PROJECT=	installer

post-extract:
	${MV} ${WRKDIR}/bootstrap ${WRKSRC}/.dotnet
	${ECHO} 'exit 0' > ${WRKSRC}/.dotnet/dotnet-install.sh

# To use this port first spin up an interactive Poudriere jail:
#   poudriere testport -i lang/dotnet6-installer
# Poudriere defines PACKAGE_BUILDING, so we make the port a no-op on the first run.
# After entering the jail, run
#   echo .undef PACKAGE_BUILDING >> /etc/make.conf
# to make the port actually build something
# You'll also need to run
#   jls
#   jail -m jid=<jail id> allow.mlock=1
# outside of the running jail for the time being.
# Once everything is set up run
#   make -C /usr/ports/lang/dotnet6-install clean all

.if defined(PACKAGE_BUILDING)
NO_BUILD=	yes
do-install:
	@${DO_NADA}
.else
do-build:
	${SETENV} HOME=${WRKDIR}/home ${WRKSRC}/build.sh
	false
.endif

.include <bsd.port.mk>
