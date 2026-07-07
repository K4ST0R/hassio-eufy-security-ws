ARG BUILD_FROM
FROM $BUILD_FROM

# Fork ws (branche) qui dépend du fork eufy-security-client (déchiffrement des clips
# d'archive ENC_EEC). Buildé automatiquement à l'install via les scripts "prepare".
ARG EUFY_SECURITY_WS_REF="github:K4ST0R/eufy-security-ws#abc1244"

WORKDIR /usr/src/app
RUN \
    set -x \
    # git : nécessaire pour installer le fork (et sa dépendance client) depuis GitHub.
    && apk add --no-cache \
        jq \
        nodejs \
        npm \
        git \
    && node --version \
    && case "$(node --version)" in v24.*) ;; *) echo "Expected Node 24, got $(node --version)" >&2; exit 1 ;; esac \
    # installe le fork ws (nom du package = "eufy-security-ws" -> même chemin qu'avant) ;
    # ses "prepare" buildent le ws ET le fork client (tsc).
    && npm install --force \
    "eufy-security-ws@${EUFY_SECURITY_WS_REF}" \
    # allège l'image : on n'a plus besoin des devDeps de build après coup
    && npm prune --omit=dev 2>/dev/null || true

COPY run.sh /
RUN chmod a+x /run.sh
WORKDIR /data

CMD [ "/run.sh" ]