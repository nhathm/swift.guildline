---
layout: default
title: SwiftLint for M1
parent: Useful script
nav_order: 1
---

# Type a script or drag a script file from your workspace to insert its path.
if [ "${CONFIGURATION}" == "Debug" ]; then
    if which swiftlint >/dev/null; then
        swiftlint
    else
        if test -d "/opt/homebrew/bin/"; then
            PATH="/opt/homebrew/bin/:${PATH}"
        fi

        export PATH
        
        if which swiftlint >/dev/null; then
            swiftlint
        else
            echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
        fi
    fi
else
    echo "SwiftLint is only running in Debug mode"
fi
