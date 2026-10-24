
build() {
    cd "${pkgbase}-${pkgver}"

    export DOTNET_CLI_HOME="$srcdir/.dotnet"
    export HOME="$srcdir/.home"
    export GIT_CONFIG_GLOBAL="$HOME/.gitconfig"
    mkdir -p "$DOTNET_CLI_HOME" "$HOME"

    rm -rf out

    local _rid="linux-x64"
    if [[ "$CARCH" == "i686" ]]; then
        _rid="linux-x86"
    fi

    local _solution="git-credential-manager.slnx"
    local _packaging="src/linux/Packaging.Linux/Packaging.Linux.csproj"

    [[ -f "$_solution" ]] || {
        printf 'ERROR: Solution not found: %s\n' "$_solution" >&2
        printf 'Available solutions:\n' >&2
        find . -maxdepth 2 \( -name '*.sln' -o -name '*.slnx' \) -print >&2
        return 1
    }

    [[ -f "$_packaging" ]] || {
        printf 'ERROR: Linux packaging project not found: %s\n' "$_packaging" >&2
        find . -name 'Packaging.Linux.csproj' -print >&2
        return 1
    }

    dotnet restore "$_solution" \
        --runtime "$_rid" \
        --disable-parallel \
        -p:Configuration=LinuxRelease \
        -p:ImportByWildcardBeforeSolution=false \
        -p:NuGetAudit=false

    dotnet build "$_packaging" \
        --no-restore \
        --configuration LinuxRelease \
        --runtime "$_rid" \
        -p:ImportByWildcardBeforeSolution=false \
        -p:NuGetAudit=false
}

check() {
    cd "${pkgbase}-${pkgver}"

    export DOTNET_CLI_HOME="$srcdir/.dotnet"
    export HOME="$srcdir/.home"
    export GIT_CONFIG_GLOBAL="$HOME/.gitconfig"
    mkdir -p "$DOTNET_CLI_HOME" "$HOME"

    local _rid="linux-x64"
    if [[ "$CARCH" == "i686" ]]; then
        _rid="linux-x86"
    fi

    LANG=C dotnet test git-credential-manager.slnx \
        --configuration LinuxRelease \
        --runtime "$_rid" \
        -p:ImportByWildcardBeforeSolution=false \
        -p:NuGetAudit=false
}
