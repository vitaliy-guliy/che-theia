# Copyright (c) 2019 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#
# Contributors:
#   Red Hat, Inc. - initial API and implementation

#{INCLUDE:docker/${BUILD_IMAGE_TARGET}/builder-from.dockerfile}

WORKDIR /home/theia

# Apply node libs installed globally to the PATH
ENV PATH=${HOME}/.yarn/bin:${PATH}

# setup extra stuff
#{INCLUDE:docker/${BUILD_IMAGE_TARGET}/builder-setup.dockerfile}
RUN yarn global add ${YARN_FLAGS} nexe@3.3.7
RUN nexe -v && \
    # Build remote binary with node runtime 12.x and che-theia node dependencies. nexe icludes to the binary only
    # necessary dependencies.
    nexe node_modules/@eclipse-che/theia-remote/lib/node/plugin-remote.js ${NEXE_FLAGS} -o ${HOME}/plugin-remote-endpoint

# Light image without node. We include remote binary to this image.
#{INCLUDE:docker/${BUILD_IMAGE_TARGET}/runtime-from.dockerfile}
#{INCLUDE:docker/${BUILD_IMAGE_TARGET}/runtime-post-env.dockerfile}

COPY --from=builder /home/theia/plugin-remote-endpoint /plugin-remote-endpoint

ENTRYPOINT cp -rf /plugin-remote-endpoint /remote-endpoint/plugin-remote-endpoint
