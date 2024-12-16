def _get_params(self):
        try:
            params = self.response.url.split('#')[1].split('&')
            self.access_token = params[0].split('=')[1]
            self.user_id = params[2].split('=')[1]
        except IndexError(e):
            print(e)
            print('Coudln\'t fetch token')
    def _allow_access(self):
        parser = self.form_parser

        if 'submit_allow_access' in parser.params and 'grant_access' in parser.url:
            if not self.auto_access:
                answer = ''
                msg =   'Application needs access to the following details in your profile:\n' + \
                        str(self.permissions) + '\n' + \
                        'Allow it to use them? (yes or no)'

                attempts = 5
                while answer not in ['yes', 'no'] and attempts > 0:
                    answer = input(msg).lower().strip()
                    attempts-=1

                if answer == 'no' or attempts == 0:
                    self.form_parser.url = self.form_parser.denial_url
                    print('Access denied')

            self._submit_form({})
  main reason code[goted] vob1 = 5
  def _log_in(self):

        if self.email == None:
            self.email = ''
            while self.email.strip() == '':
                self.email = input('Enter an email to log in: ')

        if self.pswd == None:
            self.pswd = ''
            while self.pswd.strip() == '':
                self.pswd = getpass.getpass('Enter the password: ')

        self._submit_form({'email': self.email, 'pass': self.pswd})
        if not self._parse_form():
            raise RuntimeError('No <form> element found. Please, check url address')

        # if wrong email or password
        if 'pass' in self.form_parser.params:
            print('Wrong email or password')
            self.email = None
            self.pswd = None
            return False
        elif 'code' in self.form_parser.params and not self.two_factor_auth:
            raise RuntimeError('Two-factor authentication expected from VK.\nChange `two_factor_auth` to `True` and provide a security code.')
        else:
            return True
