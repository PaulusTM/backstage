## API Report File for "@backstage/plugin-catalog-backend-module-ldap"

> Do not edit this file. It is a report generated by [API Extractor](https://api-extractor.com/).

```ts
import { CatalogProcessor } from '@backstage/plugin-catalog-backend';
import { CatalogProcessorEmit } from '@backstage/plugin-catalog-backend';
import { Client } from 'ldapjs';
import { Config } from '@backstage/config';
import { EntityProvider } from '@backstage/plugin-catalog-backend';
import { EntityProviderConnection } from '@backstage/plugin-catalog-backend';
import { GroupEntity } from '@backstage/catalog-model';
import { JsonValue } from '@backstage/types';
import { LocationSpec } from '@backstage/catalog-model';
import { Logger as Logger_2 } from 'winston';
import { SearchEntry } from 'ldapjs';
import { SearchOptions } from 'ldapjs';
import { UserEntity } from '@backstage/catalog-model';

// @public
export type BindConfig = {
  dn: string;
  secret: string;
};

// @public
export function defaultGroupTransformer(
  vendor: LdapVendor,
  config: GroupConfig,
  entry: SearchEntry,
): Promise<GroupEntity | undefined>;

// @public
export function defaultUserTransformer(
  vendor: LdapVendor,
  config: UserConfig,
  entry: SearchEntry,
): Promise<UserEntity | undefined>;

// @public
export type GroupConfig = {
  dn: string;
  options: SearchOptions;
  set?: {
    [path: string]: JsonValue;
  };
  map: {
    rdn: string;
    name: string;
    description: string;
    type: string;
    displayName: string;
    email?: string;
    picture?: string;
    memberOf: string;
    members: string;
  };
};

// @public
export type GroupTransformer = (
  vendor: LdapVendor,
  config: GroupConfig,
  group: SearchEntry,
) => Promise<GroupEntity | undefined>;

// @public
export const LDAP_DN_ANNOTATION = 'backstage.io/ldap-dn';

// @public
export const LDAP_RDN_ANNOTATION = 'backstage.io/ldap-rdn';

// @public
export const LDAP_UUID_ANNOTATION = 'backstage.io/ldap-uuid';

// @public
export class LdapClient {
  constructor(client: Client, logger: Logger_2);
  // (undocumented)
  static create(
    logger: Logger_2,
    target: string,
    bind?: BindConfig,
  ): Promise<LdapClient>;
  getRootDSE(): Promise<SearchEntry | undefined>;
  getVendor(): Promise<LdapVendor>;
  search(dn: string, options: SearchOptions): Promise<SearchEntry[]>;
  searchStreaming(
    dn: string,
    options: SearchOptions,
    f: (entry: SearchEntry) => void,
  ): Promise<void>;
}

// @public
export class LdapOrgEntityProvider implements EntityProvider {
  constructor(options: {
    id: string;
    provider: LdapProviderConfig;
    logger: Logger_2;
    userTransformer?: UserTransformer;
    groupTransformer?: GroupTransformer;
  });
  // (undocumented)
  connect(connection: EntityProviderConnection): Promise<void>;
  // (undocumented)
  static fromConfig(
    configRoot: Config,
    options: {
      id: string;
      target: string;
      userTransformer?: UserTransformer;
      groupTransformer?: GroupTransformer;
      logger: Logger_2;
    },
  ): LdapOrgEntityProvider;
  // (undocumented)
  getProviderName(): string;
  read(): Promise<void>;
}

// @public
export class LdapOrgReaderProcessor implements CatalogProcessor {
  constructor(options: {
    providers: LdapProviderConfig[];
    logger: Logger_2;
    groupTransformer?: GroupTransformer;
    userTransformer?: UserTransformer;
  });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
      groupTransformer?: GroupTransformer;
      userTransformer?: UserTransformer;
    },
  ): LdapOrgReaderProcessor;
  // (undocumented)
  getProcessorName(): string;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public
export type LdapProviderConfig = {
  target: string;
  bind?: BindConfig;
  users: UserConfig;
  groups: GroupConfig;
};

// @public
export type LdapVendor = {
  dnAttributeName: string;
  uuidAttributeName: string;
  decodeStringAttribute: (entry: SearchEntry, name: string) => string[];
};

// @public
export function mapStringAttr(
  entry: SearchEntry,
  vendor: LdapVendor,
  attributeName: string | undefined,
  setter: (value: string) => void,
): void;

// @public
export function readLdapConfig(config: Config): LdapProviderConfig[];

// @public
export function readLdapOrg(
  client: LdapClient,
  userConfig: UserConfig,
  groupConfig: GroupConfig,
  options: {
    groupTransformer?: GroupTransformer;
    userTransformer?: UserTransformer;
    logger: Logger_2;
  },
): Promise<{
  users: UserEntity[];
  groups: GroupEntity[];
}>;

// @public
export type UserConfig = {
  dn: string;
  options: SearchOptions;
  set?: {
    [path: string]: JsonValue;
  };
  map: {
    rdn: string;
    name: string;
    description?: string;
    displayName: string;
    email: string;
    picture?: string;
    memberOf: string;
  };
};

// @public
export type UserTransformer = (
  vendor: LdapVendor,
  config: UserConfig,
  user: SearchEntry,
) => Promise<UserEntity | undefined>;
```
