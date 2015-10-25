# Copyright 2015 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
# implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM karkhaz/arch-tuscan:latest
MAINTAINER Kareem Khazem <khazem@google.com>

RUN mkdir -p /home/build
RUN chown nobody: /home/build
RUN chgrp nobody /home/build
RUN chmod g+ws /home/build
RUN setfacl -m u::rwx,g::rwx /home/build

RUN echo "nobody ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN echo $'\
#!/bin/sh\n\
function build_sources ()\n\
{\n\
  trap "exit" INT\n\
  while read line1; do\n\
    echo Building $line1\n\
    cp -r /var/abs/$1/$line1 /home/build/$line1\n\
    chown nobody: /home/build/$line1\n\
    cd /home/build/$line1\n\
    sudo -u nobody makepkg --nobuild --syncdeps --rmdeps --skipinteg --skippgpcheck --skipchecksums --noconfirm --nocolor --log --noprogressbar\n\
  done\n\
}\n\
\n\
ls /var/abs/core      | build_sources core\n\
ls /var/abs/extra     | build_sources extra\n\
ls /var/abs/community | build_sources community\n\
ls /var/abs/multilib  | build_sources multilib\n\
' > /foo.sh

RUN chmod +x /foo.sh

RUN /foo.sh
