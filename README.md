echo -e '#!/bin/bash\necho "print(\"hello, world!\")" | /usr/bin/python\nexit' > execute.sh
chmod +x execute.sh
./execute.sh

mv execute.sh "execute.sh "
"./execute.sh "

echo $?
