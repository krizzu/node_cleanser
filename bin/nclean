#! /bin/sh

throwError() {
	errorMessage=$1
	echo $errorMessage >&2
}


# Perform removal
removeDir() {
	dirName=$1
	
	if [ ! -d $dirname ]; then
		throwError "$dirName is not a directory!"
		return 1
	else
		`rm -rf $dirName` 
		return 0
	fi
}

# Get list of node modules and remove them
removeNodeModules() {
	workingDir=$1
	deleteCount=0

	for dir in "${workingDir}"/*
	do
		if [ -d $dir ] && [ "$(isNodeProject $dir)" == "true" ]; then
			
			removeDir "${dir}/node_modules"
			if [ $? -eq 0 ]; then
				(( deleteCount=$deleteCount + 1 ))
			fi
		fi
	done

	echo "Deleted $deleteCount node_modules!"
}

# Check if provided dir is a node project
isNodeProject() {
	dirName=$1
	
	[ -f "${dirName}/package.json" ] && [ -d "${dirName}/node_modules" ] \
	&& echo "true" || echo "false"

}

# Find number of valid node modules + their total size
getNodeProjectsInfo() {
	workingDir=$1
	dirCount=0
	nodeModuleSize=0

	for dir in "${workingDir}"/*
	do	
		if [ -d "$dir" ]; then

			isProject=$(isNodeProject $dir)
			if [ "$isProject" == "true" ]; then
				(( dirCount=$dirCount + 1))

				moduleSize=`du -sb ${dir} | cut -f1`
				(( nodeModuleSize=$nodeModuleSize + $moduleSize ))
			fi
		fi
	done
	PROJECT_NUMBER=$dirCount
	getTotalClearSize $nodeModuleSize
}

calcSize() {
	# first arg - base 
	# second arg - exponent
	result=$(awk -v base="$1" -v exponent="$2" 'BEGIN { printf "%.2f", base / (1024 ^ exponent) }')
	echo $result
}

getTotalClearSize() {
	totalBytes=$1

	bytesInKB=1024
	bytesInMB=$(( 1024 * 1024 ))
	bytesInGB=$(( $bytesInMB * 1024 ))

	if [ $totalBytes -ge $bytesInGB ]; then
		TOTAL_CLEAR_SIZE="$(calcSize "$totalBytes" "3") GB"
	elif [ $totalBytes -ge $bytesInMB ]; then
		TOTAL_CLEAR_SIZE="$(calcSize "$totalBytes" "2") MB"
	elif [ $totalBytes -ge $bytesInKB ]; then
		TOTAL_CLEAR_SIZE="$(calcSize "$totalBytes" "1") KB"
	else
		TOTAL_CLEAR_SIZE="${totalBytes} B"
	fi
}
# You can specify what folder to check
# if not, current working directory will be used
workingDir=$1

if [ -z $workingDir ]; then
	workingDir=$PWD
else
	workingDir=`realpath $workingDir`
fi

echo "Looking for node projects in $workingDir"

PROJECT_NUMBER=0
TOTAL_CLEAR_SIZE=0

getNodeProjectsInfo $workingDir

echo
echo "Found projects : ${PROJECT_NUMBER}"
echo "Modules size   : ${TOTAL_CLEAR_SIZE}"
echo

if [ "$PROJECT_NUMBER" -ge 1 ]; then
	read -p "Clean those projects? [y/n]: " shallRemove 
		
	if [ "$shallRemove" == "y" ]; then
		removeNodeModules $workingDir
	fi
fi

echo

