IPPP Resources
--------------

Overview
~~~~~~~~

At IPPP, EOS collaboration members can use any of the following computing resources:

``login2``
  Login gateway to the IPPP infrastructure, do NOT use for any computations.

``ip3-ws4``
  40-core Intel XEON E5-2630 v4 @ 2.20GHz, 78GB RAM; shared amongst all IPPP users.

``ip3-wsXXX``
  (unavailable as of January 2023) upcoming workstation for exclusive use by EOS collaboration members.


Access
~~~~~~

Access is possible through SSH using your IPPP username (shown as ``USERNAME`` below) and password.
We recommend the following setup.

  1. If you have not yet, create a private/public SSH key pair for use between your local machine and the IPPP resources.
  (E.g. if you have a laptop and a desktop, create one pair for each.)
  It is recommended that you name the key pair files as ``id_LOCAL_ippp`` and ``id_LOCAL_ippp.pub``, where ``LOCAL`` is the name of your local machine.
  The key pair files should be stored in the ``~/.ssh`` directory.
  We recommend to use the following call to ``ssh-keygen``:
  ::

	ssh-keygen -t rsa -b 4096 -C "LOCAL -> IPPP" -f ~/.ssh/id_LOCAL_ippp

  2. Copy the contents of the public key file ``id_LOCAL_ippp.pub`` into your ``~/.ssh/authorized_keys`` file on the IPPP resources.
  This can be done using the following call to ``ssh-copy-id``:

  ::

	ssh-copy-id -i ~/.ssh/id_LOCAL_ippp.pub IPPPUSER@login2.phyip3.dur.ac.uk

  where ``LOCAL`` is the name of your local machine.

  3. If not present, create a ``~/.ssh/config`` file on your local machine and add the following lines to it:
  ::

	Host ip3-login2
		Hostname login2.phyip3.dur.ac.uk
		User IPPPUSERNAME
		IdentityFile ~/.ssh/id_LOCAL_ippp

	Host ip3-ws4
		HostName ip3-ws4.phyip3.dur.ac.uk
		ProxyCommand ssh ip3-login2 -W %h:%p
		User IPPPUSERNAME
		IdentityFile ~/.ssh/id_LOCAL_ippp

  4. You can now use ``ip3-ws4`` in lieu of the full hostname ``ip3-ws4.phyip3.dur.ac.uk``.
  Connecting to ``ip3-ws4`` is now as simple as typing ``ssh ip3-ws4``.
  Copying files to the IPPP system can be achieved by using ``scp LOCALFILE ip3-login2:REMOTEFILE``.


Setup
~~~~~

Presently, you will need to do some work to have a suitable environment to use EOS on IPPP resources.
We expect this to change once the dedicated EOS resources come online.

  - toolchain: ``gcc`` is outdate on the IPPP machines. To use ``gcc-11``, you need to load the ``devtoolset-11`` module.

  ::

	 source /opt/rh/devtoolset-11/enable


  - ``boost``: Install Boost in your home directory

  ::

	 cd ~
	 wget https://boostorg.jfrog.io/artifactory/main/release/1.79.0/source/boost_1_79_0.tar.gz
	 tar zxf boost_1_79_0.tar.gz
	 pushd boost_1_79_0
	 ./bootstrap.sh --with-python=/opt/rh/rh-python38/root/usr/bin/python3.8 --with-libraries=filesystem,python,system
	 ./b2 install --build-type=minimal --prefix=$HOME/.local
	 popd

  In your .bash_profile, add
  ::

	 export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$HOME/.local/lib"

  - EOS: When installing EOS from source, use

  ::

	./configure \
    	--with-boost-python-suffix=38 \
    	CXXFLAGS="-O2 -I$HOME/.local/include -L$HOME/.local/lib" \
    	BOOST_PYTHON_CXXFLAGS="-I$HOME/.local/include -L$HOME/.local/lib"

