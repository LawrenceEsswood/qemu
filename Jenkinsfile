// import the cheribuildProject() step
library 'ctsrd-jenkins-scripts'

def archiveQEMU(String target) {
	return {
		sh "rm -rf \$WORKSPACE/qemu-${target} && mv \$WORKSPACE/tarball/usr \$WORKSPACE/qemu-${target}"
		archiveArtifacts allowEmptyArchive: false, artifacts: "qemu-${target}/bin/qemu-system-*, qemu-${target}/share/qemu/efi-pcnet.rom, qemu-${target}/share/qemu/vgabios-cirrus.bin", fingerprint: true, onlyIfSuccessful: true
	}
}
  
cheribuildProject(target: 'qemu', cpu: 'native', skipArtifacts: true,
      buildStage: "Build Linux", nodeLabel: 'linux',
      extraArgs: '--unified-sdk --without-sdk --install-prefix=/usr --qemu/no-use-smbd',
      skipTarball: true, afterBuild: archiveQEMU('linux'))

cheribuildProject(target: 'qemu', cpu: 'native', skipArtifacts: true,
      buildStage: "Build FreeBSD", nodeLabel: 'freebsd',
      // LTO currently needs a new FreeBSD version than the one running on the slaves
      extraArgs: '--unified-sdk --without-sdk --install-prefix=/usr --qemu/no-use-lto --qemu/no-use-smbd',
      skipTarball: true, afterBuild: archiveQEMU('freebsd'))
