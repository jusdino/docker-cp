#!/bin/bash

tmpDir=/var/tmp/docker-cp
[ -d "$tmpDir" ] && rm -r "$tmpDir"
mkdir -p "$tmpDir"

case "$1" in
  *:*)
    IFS=: read sourceHost sourceVol <<<"$1"
    ;;
  *)
    sourceVol="$1"
    ;;
esac

case "$2" in
  *:*)
    IFS=: read destHost destVol <<<"$2"
    ;;
  *)
    destVol="$2"
    ;;
esac

#[[ -v sourceHost ]] && echo "Source Host: $sourceHost"
#echo "Source Volume: $sourceVol"
#[[ -v destHost ]] && echo "Dest Host: $destHost"
#echo "Dest Volume: $destVol"

echo "Retrieving source volume..."
if [[ ! -v sourceHost ]]; then
    docker run -i \
        --rm \
        -v "$sourceVol":/source \
        alpine ash -c "cd /source ; tar -cf - ." >"$tmpDir/tmpVol.tar"
else
    ssh "$sourceHost" "docker run -i --rm -v \"$sourceVol\":/source alpine ash -c \"cd /source ; tar -cf - .\"" >"$tmpDir/tmpVol.tar"
fi

echo "Creating destination volume..."
if [[ ! -v destHost ]]; then
    docker run -i \
        --rm \
        -v "$destVol":/dest \
        alpine ash -c "cd /dest ; tar -xf -" <"$tmpDir/tmpVol.tar"
else
    ssh "$destHost" "docker run -i --rm -v \"$destVol\":/dest alpine ash -c \"cd /dest ; tar -xf -\"" <"$tmpDir/tmpVol.tar"
fi

rm -r "$tmpDir"
echo "Done!"
