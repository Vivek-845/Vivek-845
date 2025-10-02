echo -e '#!/bin/bash\npython3 -c "print(\"Hello, world!\")"' > execute.sh chmod +x execute.sh
./execute.sh

mv execute.sh "execute.sh "
"./execute.sh "

echo $?
